برای اینکه فایل `values.yaml` پیش‌فرض یک Helm chart رو ببینی و تغییر بدی، مراحل زیر رو دنبال کن:

---

### **۱. دیدن مقدارهای پیش‌فرض (`values.yaml`)**

برای دیدن مقادیر پیش‌فرض یک chart، از دستور زیر استفاده کن:

```bash
helm show values <chart-name> > my-values.yaml
```

مثلاً اگر از Helm chart مربوط به NGINX در repo رسمی استفاده می‌کنی:

```bash
helm show values bitnami/nginx > my-values.yaml
```

این دستور فایل `my-values.yaml` رو ایجاد می‌کنه که همون مقادیر پیش‌فرض هست.

---

### **۲. ویرایش فایل `my-values.yaml`**

حالا می‌تونی فایل رو با هر ادیتوری باز کنی و مقادیر دلخواهت رو تغییر بدی. مثلاً:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: 1.24
```

---

### **۳. نصب یا آپدیت با استفاده از فایل ویرایش‌شده:**

**نصب:**

```bash
helm install my-release <chart-name> -f my-values.yaml
```

**آپدیت:**

```bash
helm upgrade my-release <chart-name> -f my-values.yaml
```

---

اگر repo مخصوص خودت رو داری یا chart در یک فولدر محلی هست هم می‌تونی به همین روش با `helm show values ./my-chart` یا مستقیماً فایل `values.yaml` رو در پوشه chart ببینی و تغییر بدی.
