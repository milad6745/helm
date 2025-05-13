
### ۱. استفاده از فلگ `--set` در زمان نصب یا آپدیت:

این روش برای تغییر مقادیر ساده مناسب است.

```bash
helm install my-release my-chart --set image.tag=1.2.3
```

یا اگر از قبل نصب شده و می‌خواهی آپدیت کنی:

```bash
helm upgrade my-release my-chart --set image.tag=1.2.3
```

### ۲. استفاده از فایل `values.yaml` سفارشی:

اگر مقادیر زیادی را می‌خواهی تغییر بدهی، این روش بهتر است.

مثال:

```yaml
# custom-values.yaml
image:
  repository: nginx
  tag: 1.24
replicaCount: 2
```

سپس:

```bash
helm install my-release my-chart -f custom-values.yaml
```

یا برای آپدیت:

```bash
helm upgrade my-release my-chart -f custom-values.yaml
```

### ۳. ترکیب چند مقدار با `--set`:

```bash
helm install my-release my-chart \
  --set image.repository=nginx \
  --set image.tag=1.24 \
  --set replicaCount=3
```

اگر مثلاً بخواهی مقدار در یک آرایه یا Map را تغییر بدهی، ساختار کمی پیچیده‌تر می‌شود. اگر مثالی از chart و مقداری که می‌خواهی تغییر بدهی بدهی، دقیق‌تر راهنمایی‌ات می‌کنم.
