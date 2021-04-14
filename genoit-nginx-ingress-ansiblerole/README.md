# Desplegar genoit-nginx-ingress-ansiblerole


## Resumen

Descargar el codigo ansible role del repositorio, configurar y ejecutar.

### Pasos


- El Bastion ANSIBLE debe tener instalado HELM, y configurado el KUBECTL con el cluster kubernetes a utilizar.

```raw
helm version
kubectl version
```

- El cluster KUBERNETES debe existir el NAMESPACE "ingress-nginx".

```raw
kubectl create namespace ingress-nginx
```
- Descargar y copiar el proyecto genoit-nginx-ingress-ansiblerole al Bastion ANSIBLE.

```raw
git clone -b feature/nginx-ingress codecommit::us-east-1://genoit@genoit-infrastructure-ansible
```
- Descargar y copiar el proyecto genoit-nginx-ingress-helmchart al Bastion ANSIBLE, dentro de la carpeta helmchart del proyecto genoit-nginx-ingress-ansiblerole.

```raw
- git clone -b feature/nginx-ingress-helmchart codecommit::us-east-1://genoit@genoit-infrastructure-ansible
- Copiar:
    |-- genoit-nginx-ingress-ansiblerole
        |-- helmchart
            |-- genoit-nginx-ingress-helmchart
```
- Copiar los certificados en la carpeta files del proyecto genoit-nginx-ingress-ansiblerole

```raw
- Copiar:
    |-- genoit-nginx-ingress-ansiblerole
        |-- files
            |-- ca-cert.pem
            |-- ca-key.pem            
```

- Variables ANSIBLE ROLE 

```raw
- Modificar variables importantes en el archivo main.yaml
    |-- genoit-nginx-ingress-ansiblerole
        |-- vars
            |-- main.yaml
        
- Todas las variables del HelmChart en el archivo main.yaml
    |-- genoit-nginx-ingress-ansiblerole
        |-- roles
            |-- nginx-ingress
                |-- vars
                    |-- main.yaml
```
- Desplegar el proyecto genoit-nginx-ingress-ansiblerole con ANSIBLE PLAYBOOK

```raw
- Dirigirse a la carpeta raiz del proyecto.
    |-- genoit-nginx-ingress-ansiblerole
        |-- main.yaml
        
- Ejecutar el comando con un usuario con permisos sudo en el Bastion Ansible.
  ansible-playbook main.yml
```
- Desinstalar proyecto genoit-nginx-ingress-ansiblerole

```raw       
  helm uninstall {{ desired_deployment_name }} -n {{ namespace }}
  ejm:
  helm uninstall nginx-ingress -n ingress-nginx

  kubectl -n {{ namespace }} delete secret {{ nombre_secreto_tls }} {{ desired_deployment_name }}-ingress-nginx-admission
  ejm:
  kubectl -n ingress-nginx delete secret nginx-ingress-tls nginx-ingress-ingress-nginx-admission  
```