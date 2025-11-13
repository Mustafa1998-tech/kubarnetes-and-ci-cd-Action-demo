# Kubernetes CKA Lab Guide - دليل معمل CKA

## 1️⃣ Starting Minikube and Verifying the Cluster - تشغيل Minikube والتحقق من الكلاستر

```bash          

  su - mustafa

# Start Minikube - تشغيل Minikube
minikube start

# Check nodes - التحقق من العقد
kubectl get nodes

# Check all pods in kube-system - رؤية كل الـ Pods في نظام Kubernetes
kubectl get pods -A

# Check client and server versions - التحقق من إصدارات العميل والخادم
kubectl version --client && kubectl version

# Get cluster information - الحصول على معلومات الكلاستر
kubectl cluster-info
```

## 2️⃣ Creating a Simple HTML File - إنشاء ملف HTML بسيط

```bash
# Create project directory - إنشاء مجلد المشروع
mkdir cka-demo
cd cka-demo
```

Create `index.html` with the following content:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello CKA Demo</title>
</head>
<body>
    <h1>Hello CKA Demo</h1>
</body>
</html>
```

## 3️⃣ Creating Dockerfile - إنشاء ملف Dockerfile

Create `Dockerfile` with:
```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

## 4️⃣ Building and Running Locally - بناء وتشغيل التطبيق محلياً

```bash
# Build the Docker image - بناء صورة Docker
docker build -t cka-demo:1.0 .

# List images - عرض الصور المتوفرة
docker images

# Run the container - تشغيل الحاوية
# -d: detached mode, -p: port mapping
docker run -d -p 8080:80 cka-demo:1.0
```

## 5️⃣ Deploying to Kubernetes - نشر التطبيق على Kubernetes

### 5.1 Create Deployment - إنشاء نشر
```bash
kubectl create deployment cka-demo --image=cka-demo:1.0
kubectl get pods
```

### 5.2 Expose the Deployment - جعل التطبيق متاحاً
```bash
kubectl expose deployment cka-demo --type=NodePort --port=80
kubectl get svc
```

هنا أمر واحد يفتح الصفحة مباشرة من الـ Terminal:

xdg-open http://$(minikube ip):$(kubectl get svc cka-demo -o jsonpath='{.spec.ports[0].nodePort}')


minikube ip بيجيب الـ IP الخاص بالـ Minikube.

kubectl get svc ... jsonpath بيجيب رقم الـ NodePort تلقائياً.

xdg-open يفتح الرابط في المتصفح تلقائياً على لينكس/WSL.

بعد ما تشغل الأمر ده، المتصفح هيفتح صفحة Hello CKA Demo مباشرة. 🚀

### 5.3 Access the Application - الوصول للتطبيق
```bash
minikube service cka-demo
```

## 6️⃣ Scaling the Application - تكبير حجم التطبيق

```bash
# Scale to 3 replicas - تكبير إلى 3 نسخ
kubectl scale deployment cka-demo --replicas=3

# Verify pods - التحقق من الـ Pods
kubectl get pods

# Check deployment status - التحقق من حالة النشر
kubectl get deployments
```

## 7️⃣ Autoscaling - التكبير التلقائي

```bash
# Create Horizontal Pod Autoscaler - إنشاء مقياس آلي
kubectl autoscale deployment cka-demo --cpu-percent=50 --min=3 --max=5

# Check HPA status - التحقق من حالة المقياس الآلي
kubectl get hpa
```

## 8️⃣ Updates and Rollbacks - التحديثات والتراجع

```bash
# Update the image - تحديث الصورة
kubectl set image deployment/cka-demo cka-demo=cka-demo:1.1

# Check rollout status - التحقق من حالة التحديث
kubectl rollout status deployment/cka-demo

# Rollback if needed - التراجع عن التحديث إذا لزم الأمر
kubectl rollout undo deployment/cka-demo
```

## 9️⃣ Cleanup - التنظيف

```bash
# Delete resources - حذف الموارد
kubectl delete deployment cka-demo
kubectl delete svc cka-demo
kubectl delete hpa cka-demo

# Stop Minikube - إيقاف Minikube
minikube stop
```
## 🔍 Verification - التحقق

1. After deployment, access the service URL and verify you see "Hello CKA Demo"
   - بعد النشر، تأكد من رؤية "Hello CKA Demo" عند الدخول على عنوان الخدمة

2. View all resources - رؤية كل الموارد
   ```bash
   kubectl get all
   ```

3. Check logs - رؤية السجلات
   ```bash
   kubectl logs -l app=cka-demo
   ```

## 📝 Notes - ملاحظات

- All commands should be run from the project directory
- Make sure Docker is running before starting Minikube
- Use `minikube dashboard` for a web-based Kubernetes UI

## 📚 Additional Resources - موارد إضافية

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
