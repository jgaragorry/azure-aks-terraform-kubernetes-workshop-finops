# 🚀 Azure AKS Kubernetes Workshop con Terraform y FinOps

[![Terraform](https://img.shields.io/badge/Terraform-automated-5C4EE5?style=for-the-badge&logo=terraform)](#)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-hands--on-326CE5?style=for-the-badge&logo=kubernetes)](#)
[![Azure AKS](https://img.shields.io/badge/Azure-AKS-0078D4?style=for-the-badge&logo=microsoft-azure)](#)
[![FinOps](https://img.shields.io/badge/FinOps-cost%20optimized-00A1C9?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-000?style=for-the-badge)](#)

---

## 🎯 Objetivo del Workshop

Desplegar un clúster Kubernetes en Azure AKS usando Terraform, publicar una aplicación demo (Nginx), habilitar escalado automático (HPA) y validar que funciona, aplicando buenas prácticas de DevOps y FinOps.

---

## 📑 Índice

1. Prerrequisitos  
2. Arquitectura del Workshop  
3. Orden de ejecución  
4. Estimación de costos  
5. Buenas prácticas aplicadas  
6. Resultados esperados  
7. Cleanup

---

## 🔧 Prerrequisitos

Instala estas herramientas en tu PC:

- Azure CLI  
- Terraform  
- kubectl  
- Git

Clona el repositorio:

git clone https://github.com/jgaragorry/azure-aks-terraform-kubernetes-workshop-finops.git  
cd azure-aks-terraform-kubernetes-workshop-finops

---

## 🏗 Arquitectura del Workshop

- Backend remoto de Terraform → Azure Storage (separado del clúster)  
- Infraestructura → Resource Group, VNet, AKS con 2 nodos Standard_B2s  
- Aplicación demo → Nginx con Service tipo LoadBalancer  
- Escalado automático → HPA basado en CPU  
- Tagging FinOps → etiquetas en recursos cloud y Kubernetes

---

## 📜 Orden de ejecución

1. Configurar variables:

cp scripts/env.example .env  
source .env

2. Crear backend remoto:

bash scripts/az-backend-create.sh  
bash scripts/tf-init.sh

3. Desplegar infraestructura AKS:

bash scripts/tf-validate-plan.sh  
bash scripts/tf-apply.sh

4. Desplegar aplicación demo:

bash scripts/kube-deploy.sh  
bash scripts/kube-verify.sh

5. Verificar funcionamiento:

kubectl -n workshop get svc workshop-demo-svc

Abre la IP pública en tu navegador → debe mostrar la página de Nginx.

Genera carga para ver el HPA en acción:

kubectl -n workshop run load -it --rm --image=busybox --restart=Never -- wget -qO- http://<IP>

---

## 💰 Estimación de costos

- AKS: 2 nodos Standard_B2s → ~USD 0.24–0.30/h  
- Workshop de 3 horas → ~USD 0.75 por persona  
- FinOps aplicado → destruir recursos al terminar, usar nodos mínimos, etiquetar todo

---

## ✅ Buenas prácticas aplicadas

- Separación del tfstate → backend remoto en Azure Storage  
- Automatización completa → scripts y Makefile (init, validate, plan, apply, destroy)  
- Tagging FinOps → env, owner, project, cost-center, lifecycle  
- Límites de recursos en pods → requests/limits de CPU y memoria  
- Cleanup controlado → destrucción segura sin perder tfstate

---

## 📊 Resultados esperados

- Clúster AKS activo con 2 nodos  
- App Nginx accesible vía IP pública  
- HPA escalando pods bajo carga  
- Evidencia tangible: salida de kubectl get deploy,svc,hpa,pods

---

## 🧹 Cleanup

bash scripts/kube-cleanup.sh  
bash scripts/tf-destroy.sh  
bash scripts/az-backend-destroy.sh

---

## 🧠 Valor para LinkedIn

Título sugerido:  
“De cero a Kubernetes en Azure en 2 horas: Terraform + AKS + FinOps”

Hook:  
“Menos de USD 1 para practicar Kubernetes con evidencias reales.”

CTA:  
“¿Quieres reproducirlo? Aquí está el repo.”

Hashtags:  
#Kubernetes #AKS #Terraform #DevOps #FinOps #CloudEngineering
