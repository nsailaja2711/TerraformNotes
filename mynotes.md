**Basic Folder structure should be as mentioned below**

terraform-ec2/
├── main.tf              # root module
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── modules/
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
