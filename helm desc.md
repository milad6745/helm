بریم Helm رو قدم به قدم خیلی شسته‌رفته‌تر باز کنیم:

---

# 🚀 Helm چیه دقیقاً؟

**Helm** یه ابزار مدیریت package برای **Kubernetes** هست.

➔ درست مثل apt برای Ubuntu یا npm برای Node.js  
➔ ولی برای Kubernetes: manifest ها رو مدیریت می‌کنه.

---

# 🎯 Helm دقیقا چه مشکلی حل می‌کنه؟

قبل از Helm:

✅ اگه میخواستی یه سرویس deploy کنی، باید ۱۰-۲۰ تا فایل YAML جدا جدا بنویسی (deployment، service، ingress، configmap، secret و...)  
✅ هر بار هم که یه تغییر کوچیک میدادی باید دستی همه رو sync میکردی  
✅ و برای محیط‌های مختلف (dev, staging, prod) باید فایل‌های YAML مختلف نگه میداشتی (کابوس 😬)

➔ **Helm** اومد اینو درست کرد:
- همه چیز رو یه جا بستبندی میکنه
- پارامتریک میکنه (با values.yaml)
- version control داره
- upgrade و rollback راحت میده

---

# 🧩 ساختار یک Helm Chart

وقتی میگی "Chart"، یعنی یه بسته‌ی قابل deploy روی Kubernetes.

داخل یک Chart معمولا اینارو داریم:

| فایل / پوشه | وظیفه |
|-------------|-------|
| Chart.yaml | مشخصات کلی Chart (اسم، نسخه، توضیح) |
| values.yaml | تنظیمات پیش‌فرض قابل override |
| templates/ | فایل‌های YAML با قابلیت جایگذاری متغیر |
| charts/ | وابستگی‌ها (subcharts) |
| README.md | توضیحات |

---

# 📜 Chart.yaml نمونه

```yaml
apiVersion: v2
name: my-app
description: A Helm chart for deploying my awesome app
type: application
version: 0.1.0
appVersion: "1.0.0"
```

---

# 📜 values.yaml نمونه

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: stable

service:
  type: ClusterIP
  port: 80
```

---

# 📜 templates/deployment.yaml نمونه

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
      - name: {{ .Release.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: {{ .Values.service.port }}
```

➔ اینجا می‌بینی که با `{{ .Values.xxx }}` و `{{ .Release.Name }}` به صورت داینامیک YAML ساخته میشه. 😎

---

# 🛠️ دستورات اصلی Helm

| دستور | توضیح |
|-------|-------|
| `helm install myapp ./mychart` | نصب یک chart |
| `helm upgrade myapp ./mychart` | ارتقای نسخه یا تغییر تنظیمات |
| `helm uninstall myapp` | حذف chart |
| `helm list` | دیدن chart های نصب شده |
| `helm package ./mychart` | بسته‌بندی chart به فایل `.tgz` |
| `helm repo add stable https://charts.helm.sh/stable` | اضافه کردن ریپازیتوری chart |
| `helm pull stable/mysql` | کشیدن chart آماده |
| `helm template ./mychart` | تولید manifest بدون نصب (خروجی خام YAML) |

---

# 💡 مفاهیم پیشرفته‌تر Helm

- **Subcharts** ➔ وقتی یک chart دیگه داخل chart اصلیت نیاز داری
- **Dependencies** ➔ declare وابستگی‌ها تو Chart.yaml
- **Helm Hooks** ➔ اجرای دستورات قبل یا بعد از install/upgrade/delete
- **Helm Test** ➔ تست کردن بعد از deploy
- **Helm Secrets** ➔ رمزنگاری مقادیر حساس داخل values.yaml

---

# ✨ Helm vs Kustomize؟

| ویژگی | Helm | Kustomize |
|-------|------|-----------|
| Template Engine | دارد (Go template) | ندارد (Patch-based) |
| Packaging (tgz) | دارد | ندارد |
| Version Control روی Chart | دارد | ندارد |
| مناسب برای Apps Complex | خیلی خوب | خوب برای ساده و medium apps |
| یادگیری اولیه | کمی پیچیده‌تر | راحت‌تر |

➔ معمولا پروژه‌های serious میرن سمت Helm چون versioned + parametrized راحت داره.

---

# 📈 Helm توی GitOps؟

بله!  
با Flux یا ArgoCD میتونی Helm Chart رو به عنوان source مستقیم Deploy کنی.

مثلا:

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: myapp
  namespace: apps
spec:
  releaseName: myapp
  chart:
    spec:
      chart: ./charts/myapp
      sourceRef:
        kind: GitRepository
        name: my-repo
        namespace: flux-system
  interval: 5m
  values:
    replicaCount: 3
```

---

