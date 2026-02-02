# Terraform Modules – Interview Ready Guide

## 1. What is a Terraform Module?

A **Terraform module** is a container for multiple resources that are used together. Technically, **any folder containing `.tf` files is a module**.

* **Root module**: The main working directory where you run `terraform init/apply`
* **Child module**: A reusable module called from the root module using `module` block

👉 Interview one‑liner:

> A Terraform module is a reusable, parameterized set of Terraform configurations used to standardize and manage infrastructure.

---

## 2. Why Do We Use Modules?

* Reusability (write once, use many times)
* Cleaner code (separation of concerns)
* Consistency across environments
* Easy maintenance
* Industry best practice for large infra

👉 Interview answer:

> Modules help reduce duplication, enforce standards, and simplify infrastructure management across multiple environments.

---

## 3. Types of Modules

1. **Root Module** – current working directory
2. **Child Module** – called using `module` block
3. **Local Module** – stored locally (`./modules/ec2`)
4. **Remote Module** – from Git, Terraform Registry, S3

Example:

```hcl
module "ec2" {
  source = "./modules/ec2"
}
```

---

## 4. Standard Module Structure (Best Practice)

```
modules/
└── ec2/
    ├── main.tf        # resources
    ├── variables.tf   # inputs
    ├── outputs.tf     # outputs
```

---

## 5. How Data Flows in Terraform Modules

```
tfvars → root variables → module inputs → resources → outputs
```

Explanation:

* Values are passed from `terraform.tfvars`
* Root module variables receive them
* Values are passed into child module
* Resources use them
* Outputs expose important values

---

## 6. Variables in Modules

### variables.tf

```hcl
variable "instance_type" {
  type        = string
  description = "EC2 instance type"
}
```

Key points:

* Variables make modules reusable
* Support types: string, number, bool, list, map, object

👉 Interview question:
**Q:** Why variables are important in modules?
**A:** They allow parameterization so the same module can be reused with different configurations.

---

## 7. Calling a Module (Root Module)

```hcl
module "ec2" {
  source        = "./modules/ec2"
  instance_type = var.instance_type
}
```

Important:

* `source` is mandatory
* Values should come from variables, not hard-coded

---

## 8. tfvars and Modules (VERY IMPORTANT)

### terraform.tfvars

```hcl
instance_type = "t2.micro"
```

Terraform automatically loads:

* `terraform.tfvars`
* `*.auto.tfvars`

For multiple environments:

```bash
terraform apply -var-file=dev.tfvars
terraform apply -var-file=prod.tfvars
```

👉 Interview question:
**Q:** Can we pass values to modules using tfvars?
**A:** Yes. tfvars pass values to root variables, which are then forwarded to modules.

---

## 9. Outputs in Modules

### outputs.tf

```hcl
output "public_ip" {
  value = aws_instance.this.public_ip
}
```

Access from root:

```hcl
module.ec2.public_ip
```

Why outputs?

* Share data between modules
* Display important information

---

## 10. Module Versioning

Remote module example:

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"
}
```

Best practice:

* Always pin module versions

---

## 11. count vs for_each in Modules

* `count` → index based (not stable)
* `for_each` → key based (recommended)

Example:

```hcl
for_each = var.instances
```

👉 Interview preference: **for_each**

---

## 12. Common Interview Questions & Answers

### Q1. Is a module mandatory in Terraform?

❌ No. But it is recommended for real projects.

### Q2. Can a module have multiple resources?

✅ Yes. A module can contain any number of resources.

### Q3. Where should provider blocks be defined?

✅ In root module (best practice)

### Q4. Can modules call other modules?

✅ Yes (nested modules)

### Q5. Can we hard-code values inside modules?

❌ Avoid it. Use variables.

---

## 13. Real-Time Production Example

* One EC2 module
* Used by dev, qa, prod
* Different tfvars
* Same module code

This ensures:

* Zero duplication
* Easy scaling
* Safe deployments

---

## 14. One-Line Summary (Interview Gold)

> Terraform modules are reusable, parameter-driven building blocks that standardize infrastructure creation across environments.

---

## 15. Final Tip for Interviews ❤️

If stuck, always say:

> "We use modules to make Terraform code reusable, scalable, and easy to maintain across environments using tfvars."

That answer **never fails**.
