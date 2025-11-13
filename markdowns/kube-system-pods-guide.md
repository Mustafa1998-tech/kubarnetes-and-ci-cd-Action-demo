# Understanding `kubectl get pods -n kube-system`

## Command Overview / نظرة عامة على الأمر

```bash
kubectl get pods -n kube-system
```

## Explanation / الشرح

### Command Breakdown / تحليل الأمر
- `kubectl` - The Kubernetes command-line tool
- `get pods` - Lists all pods in the current namespace
- `-n kube-system` - Specifies the system namespace where Kubernetes components run

### What is kube-system? / ما هو kube-system؟
The `kube-system` namespace is a special namespace in Kubernetes that contains system components and add-ons that are essential for the cluster's operation.

## Expected Output / المخرجات المتوقعة

```
NAME                                     READY   STATUS    RESTARTS   AGE
coredns-7db6d8ff4d-b7fwh                 1/1     Running   0          10m
coredns-7db6d8ff4d-mn9kx                 1/1     Running   0          10m
etcd-minikube                            1/1     Running   0          10m
kube-apiserver-minikube                  1/1     Running   0          10m
kube-controller-manager-minikube         1/1     Running   0          10m
kube-proxy-8hsdf                         1/1     Running   0          10m
kube-scheduler-minikube                  1/1     Running   0          10m
```

## Column Descriptions / وصف الأعمدة

| Column / العمود | Description / الوصف |
|----------------|-------------------|
| **NAME** | اسم الـ Pod |
| **READY** | عدد الحاويات الجاهزة / العدد الكلي للحاويات في الـ Pod |
| **STATUS** | حالة الـ Pod (مثل: Running, Pending, CrashLoopBackOff) |
| **RESTARTS** | عدد مرات إعادة تشغيل الحاوية بسبب فشل |
| **AGE** | المدة المنقضية منذ إنشاء الـ Pod |

## Key Components in kube-system / المكونات الرئيسية في kube-system

### Core Components / المكونات الأساسية
1. **etcd**
   - Key-value store for cluster data
   - يخزن جميع بيانات الكلستر

2. **kube-apiserver**
   - Frontend for the Kubernetes control plane
   - واجهة برمجة التطبيقات للتحكم في الكلستر

3. **kube-controller-manager**
   - Runs controller processes
   - يدير العمليات المسؤولة عن الحفاظ على الحالة المطلوبة

4. **kube-scheduler**
   - Schedules pods to nodes
   - يحدد العقدة التي ستعمل عليها الـ Pods

5. **kube-proxy**
   - Network proxy and load balancer
   - يدير الاتصالات الشبكية بين الـ Pods

6. **CoreDNS**
   - Cluster DNS server
   - يوفر خدمة DNS داخل الكلستر

## Common Issues / المشاكل الشائعة

1. **Pod in CrashLoopBackOff**
   - Check logs: `kubectl logs <pod-name> -n kube-system`
   - Describe pod: `kubectl describe pod <pod-name> -n kube-system`

2. **Pod Pending**
   - Check node resources: `kubectl describe nodes`
   - Check events: `kubectl get events -n kube-system`

3. **Pod Not Ready**
   - Check container status: `kubectl get pods -n kube-system -o wide`
   - Check container logs: `kubectl logs <pod-name> -n kube-system -c <container-name>`

## Useful Commands / أوامر مفيدة

```bash
# Get detailed information about a pod
kubectl describe pod <pod-name> -n kube-system

# View logs of a specific container
kubectl logs <pod-name> -n kube-system -c <container-name>

# Get pod information in wide format
kubectl get pods -n kube-system -o wide

# Watch pods in real-time
kubectl get pods -n kube-system -w
```
