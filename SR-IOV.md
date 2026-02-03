# MDE – Giám sát OpenShift (SR-IOV) cho hệ thống Telecom (MME/PGW/UPF)

## 1. Mục đích (Purpose)

Tài liệu này mô tả **quy trình giám sát OpenShift** tập trung vào **Network hiệu năng cao sử dụng SR-IOV**, nhằm:

* Phát hiện sớm suy giảm hiệu năng (latency, packet loss, throughput)
* Correlate KPI Telecom (ASR, CSSR, Session Success) với tài nguyên Cloud
* Chuẩn hoá thao tác cho đội **Vận hành (Ops)**

## 2. Phạm vi (Scope)

Áp dụng cho:

* OpenShift Container Platform (OCP)
* Workload Telecom: **MME / PGW / UPF / IMS**
* Network CNI: **SR-IOV + Multus**
* Monitoring: **Prometheus, Alertmanager, Grafana**

Không áp dụng cho môi trường non-SR-IOV (bridge/overlay thuần).

## 3. Kiến trúc tổng thể (High-level Architecture)

```
[Pod Telecom]
     |
     | (net1 – SR-IOV VF)
     v
[Virtual Function (VF)]
     |
     v
[Physical NIC]
     |
     v
[Switch / Transport Network]

Metrics flow:
NIC/VF → Node → Prometheus → Alertmanager → Telegram/Email
                         → QuestDB → Dashboard/KPI
```

## 4. Thành phần giám sát (Monitoring Components)

### 4.1 OpenShift Built-in Monitoring

* Prometheus (metrics collection)
* Alertmanager (alert routing)
* Grafana (dashboard)
* kube-state-metrics
* node-exporter

### 4.2 SR-IOV Components

* SR-IOV Network Operator
* SR-IOV Device Plugin
* SR-IOV CNI

## 5. Điều kiện tiên quyết (Pre-check)

### 5.1 Kiểm tra SR-IOV Operator

```bash
oc get pods -n openshift-sriov-network-operator
```

Yêu cầu tất cả pod ở trạng thái **Running**.

### 5.2 Kiểm tra Virtual Function trên Node

```bash
oc get sriovnetworknodestates -n openshift-sriov-network-operator
```

Hoặc trực tiếp trên node:

```bash
ip link show
```

Phải tồn tại các interface dạng `ensXXXv0`, `ensXXXv1`.

### 5.3 Kiểm tra Pod attach SR-IOV

```bash
oc describe pod <pod-name>
```

Xác nhận annotation:

```
k8s.v1.cni.cncf.io/networks: sriov-net
```

## 6. Quy trình giám sát (Monitoring Procedure)

### 6.1 Giám sát Node & NIC (Layer vật lý)

**Mục tiêu:** phát hiện overload phần cứng.

* CPU / RAM / Disk node
* NIC throughput

PromQL tham khảo:

```
rate(node_network_receive_bytes_total[5m])
rate(node_network_transmit_bytes_total[5m])
```

### 6.2 Giám sát SR-IOV VF (Layer mạng)

**Mục tiêu:** phát hiện packet drop, lỗi VF.

Kiểm tra trực tiếp trên node:

```bash
ethtool -S <vf-interface>
```

Theo dõi các counter:

* rx_packets
* tx_packets
* rx_dropped
* tx_dropped

Nếu cluster expose metrics SR-IOV, dùng PromQL:

```
rate(sriov_vf_rx_dropped[5m])
rate(sriov_vf_tx_packets[5m])
```

### 6.3 Giám sát Pod Telecom (Layer ứng dụng)

* Pod restart count
* Pod CPU / Memory
* Pod readiness

PromQL:

```
kube_pod_container_status_restarts_total
container_cpu_usage_seconds_total
container_memory_working_set_bytes
```

### 6.4 Correlate KPI Telecom

Kết hợp:

* Network metrics (VF drop, throughput)
* Pod metrics (restart, resource)
* KPI Telecom (ASR, CSSR, Session Success)

Mục tiêu: xác định **root cause** (Network / Pod / Node).

## 7. Cảnh báo (Alerting Rules)

### 7.1 Network Alert

* VF rx_dropped > 0 trong 5 phút
* NIC utilization > 80%

### 7.2 Pod Alert

* Pod restart > 3 lần / 5 phút
* Pod Pending > 3 phút

### 7.3 KPI Alert

* ASR/CSSR giảm vượt ngưỡng cấu hình

Alert flow:

```
Prometheus → Alertmanager → Telegram / Email
```

## 8. Xử lý sự cố (Troubleshooting)

### 8.1 Packet drop tăng

1. Kiểm tra VF counter
2. Kiểm tra NIC physical
3. Kiểm tra switch / uplink
4. Đối soát KPI Telecom

### 8.2 Pod latency cao

1. Kiểm tra CPU/Memory pod
2. Kiểm tra network VF latency
3. Kiểm tra node overload

## 9. Logging & Lưu trữ dữ liệu

* Metrics ngắn hạn: Prometheus
* Metrics dài hạn: Thanos / QuestDB
* KPI Telecom: QuestDB

## 10. Kết luận

Quy trình giám sát OpenShift với SR-IOV giúp:

* Phát hiện sớm lỗi network khó nhìn thấy
* Liên kết Cloud metrics với KPI Telecom
* Nâng cao độ ổn định cho hệ thống MME/PGW

---

**Tài liệu này được sử dụng làm chuẩn vận hành (MDE) cho đội Ops.**
