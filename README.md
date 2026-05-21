# Actuación en clase - Terraform y GitHub Actions

> **¿Por qué "Terrorform"?**
> No es un typo. Es una advertencia. Este repositorio simula el escenario clásico de heredar infraestructura rota: errores sutiles en Terraform y en el pipeline de CI/CD que hacen que nada funcione. Tu misión: convertir el _terror_ en Terraform.

## Objetivo

Diagnosticar y corregir los problemas en un pipeline de CI/CD existente que utiliza GitHub Actions y Terraform para desplegar infraestructura en AWS de forma funcional.

## Contexto

Se te ha entregado un repositorio con infraestructura definida en Terraform y un pipeline de CI/CD implementado con GitHub Actions. Sin embargo, tanto el pipeline como los archivos de Terraform tienen varios problemas que debes identificar y corregir para que el despliegue funcione correctamente.

## Arquitectura objetivo

```
Internet → IGW → ALB (subnets públicas) → EC2 vía ASG (subnets privadas)
                                                 ↑
                                           NAT Gateway (subnet pública)
```

## Estructura del repositorio

```
├── .github/
│   └── workflows/
│       └── cicd.yaml       # Pipeline CI/CD (tiene errores)
├── alb.tf                  # Application Load Balancer
├── ec2.tf                  # Launch Template y Auto Scaling Group
├── networking.tf           # VPC, subnets, IGW, NAT Gateway, route tables
├── outputs.tf              # Outputs de Terraform
├── provider.tf             # Configuración del provider y backend
├── security_groups.tf      # Security Groups para ALB y EC2
├── user_data.sh            # Script de inicialización de las instancias
└── README.md               # Este archivo
```


## Recursos desplegados

| Archivo | Recursos |
|---------|----------|
| `networking.tf` | VPC, IGW, 4 subnets (2 públicas / 2 privadas), NAT GW, EIP, route tables |
| `security_groups.tf` | SG para ALB (HTTP 80 desde internet), SG para EC2 (HTTP 80 desde ALB) |
| `alb.tf` | Application Load Balancer, Target Group, Listener HTTP:80 |
| `ec2.tf` | Launch Template, Auto Scaling Group (min/max/desired: 2) |
| `provider.tf` | AWS provider `us-east-1` |
| `outputs.tf` | DNS del ALB |

## Evaluación

Tu solución se evaluará según los siguientes criterios:

1. El pipeline se ejecuta correctamente sin errores
2. Todos los problemas identificados han sido corregidos
3. La infraestructura se despliega correctamente en AWS
4. El ALB responde con HTTP 200 desde internet
5. Las instancias EC2 solo reciben tráfico desde el ALB (no desde internet directamente)


## Definición de terminado

El resultado debe ser la visualización del sitio web accediendo desde la URL del ALB impresa en el summary del workflow. Cada EC2 responde con su propio `Instance ID` y `Hostname`, confirmando que el ALB distribuye el tráfico entre ambas instancias del ASG.

## Entrega

1. Identifica y corrige todos los problemas en los archivos del repo
2. Haz commit y push de tus cambios
3. Verifica que el pipeline funcione correctamente en la pestaña "Actions"
4. Confirma que el ALB URL impreso en el summary responde correctamente

¡Buena suerte!
