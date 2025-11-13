# CKA Demo Lab: Nginx Deployment & Service

## 1️⃣ Create Deployment / إنشاء Deployment

File: `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

**Command to apply:**
```bash
kubectl apply -f deployment.yaml
```

**Check the pods:**
```bash
kubectl get pods
```
**ملاحظة:** يجب أن يكون `READY` بقيمة `1/1` لكل Pod عند التشغيل.

## 2️⃣ Create NodePort Service / إنشاء Service من نوع NodePort

File: `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-deploy
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

**Command to apply:**
```bash
kubectl apply -f service.yaml
```

**Check the service:**
```bash
kubectl get svc
```
**ملاحظة:** سترى عنوان IP داخلي و NodePort مثل `31825`.

**To access the service in browser (with minikube):**
```bash
minikube service nginx-deploy
```

## 3️⃣ Update Deployment Version / تحديث نسخة الـ Deployment

**Change Nginx image:**
```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.29.3
```

**Check rollout status:**
```bash
kubectl rollout status deployment/nginx-deploy
kubectl get pods
```

**Rollback update:**
```bash
kubectl rollout undo deployment/nginx-deploy
```

## 4️⃣ Create ConfigMap for HTML / إنشاء ConfigMap

File: `configmap.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-html-config
data:
  index.html: |
    <html>
      <head><title>CKA Demo</title></head>
      <body>
        <h1>Hello CKA Demo!</h1>
      </body>
    </html>
```

**Command to apply:**
```bash
kubectl apply -f configmap.yaml
```

## 5️⃣ Update Deployment to use ConfigMap / تحديث Deployment لاستخدام ConfigMap

File: `deployment-with-configmap.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: html-volume
        configMap:
          name: nginx-html-config
```

**Apply the update:**
```bash
kubectl apply -f deployment-with-configmap.yaml
kubectl get pods
```

## 6️⃣ Verification / التحقق من التفاصيل

**Get pod details:**
```bash
kubectl describe pod <pod-name>
```

**View pod logs:**
```bash
kubectl logs <pod-name>
```

## 🔹 Important Notes / ملاحظات مهمة

- `kubectl get pods -w` - Watch pod status in real-time
- `kubectl get deployments` - Check deployment status and available replicas
- ConfigMaps are useful for changing configuration files without rebuilding the image
- For production, consider adding resource requests and limits

## 🚀 Cleanup / التنظيف

To delete all created resources:
```bash
kubectl delete -f deployment-with-configmap.yaml
kubectl delete -f service.yaml
kubectl delete -f configmap.yaml
```
