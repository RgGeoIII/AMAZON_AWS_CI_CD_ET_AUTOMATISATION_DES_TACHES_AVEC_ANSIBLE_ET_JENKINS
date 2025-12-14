# ☁️ AMAZON AWS : CI / CD ET AUTOMATISATION DES TÂCHES AVEC ANSIBLE ET JENKINS

Ce projet pédagogique présente une **approche pratique** pour mettre en place une chaîne **CI/CD** et automatiser la configuration et le déploiement d’applications sur **Amazon Web Services (AWS)** en utilisant **Ansible** pour la configuration et **Jenkins** pour l’orchestration du pipeline.

---

## 📁 Structure du projet

- `infrastructure/` : templates & scripts de provisionnement (CloudFormation / Terraform / scripts bash)
- `ansible/` : playbooks, rôles et inventaires Ansible
- `jenkins/` : Jenkinsfile(s), jobs et scripts d’intégration
- `app/` : exemple d’application (frontend/backend) servant de cible au déploiement
- `scripts/` : utilitaires (provision, recovery-ebs, snapshot, etc.)
- `docs/` : architecture, runbooks, procédures (recuperation d’accès, rollback, sécurité)
- `tests/` : suites de tests unitaires / d’intégration
- `README.md` : ce fichier
- `LICENSE` : licence du projet

---

## 🎯 Objectifs pédagogiques

- Comprendre les composants clefs d’une pipeline CI/CD sur AWS.
- Automatiser la configuration des serveurs et le déploiement d’applications avec **Ansible**.
- Orchestrer les builds, tests et déploiements avec **Jenkins** (webhooks, credentials, stages).
- Mettre en place des runbooks de récupération (EBS / SSM) et des bonnes pratiques de sécurité pour l’infrastructure cloud.
- Proposer des exemples réutilisables pour un environnement de type stage/prod.

---

## ⚠️ DISCLAIMER & ÉTHIQUE

> Ce projet est strictement pédagogique et opérationnel dans un contexte de tests ou d’environnement contrôlé.

- ❌ **Ne jamais** publier de clés privées (`.pem`) ni de secrets dans le dépôt.
- ✅ Respecte le **principe du moindre privilège** (IAM) et utilise des rôles quand c’est possible.
- 🔒 Stocke les secrets dans **Jenkins Credentials**, **AWS Secrets Manager** ou **Ansible Vault**.
- Toute utilisation en production doit être précédée d’un audit sécurité.

---

## 🛠️ Prérequis

- Compte AWS avec droits pour EC2, IAM, VPC, EBS (ou accès via rôle).
- AWS CLI configuré (`aws configure`) ou accès via IAM Role.
- Ansible (>=2.9).
- Jenkins (stable) + plugins recommandés : Git, Pipeline, Ansible, SSH Credentials, Credentials Binding.
- Git, Docker (optionnel), et une connaissance basique des systèmes Linux/SSH.

---

## 🏗️ Exemple d’architecture cible

- VPC dédié avec subnets publics/privés
- Security Groups restreints (SSH limité, HTTP/HTTPS pour web)
- EC2 : `jenkins` (optionnel), `app` (cible de déploiement)
- Jenkins déclenché par webhook Git → exécute tests/build → playbooks Ansible pour déployer sur EC2
- Monitoring basique via CloudWatch / logs centralisés

---

## 🧭 Workflow résumé

1. Développement → Push sur Git (branche `main` ou `staging`).
2. Webhook déclenche Jenkins Pipeline.
3. Jenkins exécute tests → build (optionnel : image Docker) → artefacts.
4. Jenkins appelle Ansible (via CLI ou plugin) pour déployer sur EC2.
5. Post-déploiement : smoke tests, notifications, rollback si erreur.

---

## ⚙️ Exemples de fichiers utiles

- `ansible/playbooks/site.yml` — playbook principal (rôles : `common`, `web`, `app`)
- `inventory/hosts.ini` — inventaire statique ou dynamique (EC2 dynamic inventory)
- `jenkins/Jenkinsfile` — pipeline declarative (checkout → test → build → deploy)
- `scripts/provision-ec2.sh` — bootstrap d’une instance (cloud-init / userdata)
- `docs/runbooks.md` — procédures : récupération accès (SSM / EBS), sauvegardes, snapshots

---

## ✅ Bonnes pratiques & sécurité

- Utiliser **Ansible Vault** pour chiffrer secrets dans les playbooks.
- Préférer **IAM Roles** pour Jenkins quand il est exécuté sur EC2 plutôt que des clés statiques.
- Restreindre les accès SSH (IP whitelisting, jumpbox).
- Automatiser les snapshots EBS et les AMI avant déploiement majeur.
- Mettre en place des tests automatisés (unit / integration / smoke) pour valider chaque déploiement.
- Centraliser logs et alertes (CloudWatch, ELK, Prometheus/Grafana).

---

## 🩺 Runbook : perte d’accès SSH (résumé)

- **SSM** : si l’agent SSM est installé et le rôle IAM présent → Session Manager depuis la console AWS.
- **EBS recovery** : arrêter l’instance → détacher le volume root → attacher à une instance de secours → modifier `/home/<user>/.ssh/authorized_keys` → rattacher → redémarrer.
- **Créer AMI** : snapshot/AMI de l’instance → lancer une nouvelle instance en choisissant la nouvelle paire de clés.

> Voir `docs/runbooks.md` pour la procédure pas-à-pas.

---

## 🧪 Tests & validation

- `ansible all -m ping -i inventory/hosts.ini` pour tester la connectivité.
- `ansible-playbook --syntax-check ansible/playbooks/site.yml` pour la vérification de syntaxe.
- Intégrer un stage `Tests` dans le Jenkinsfile pour exécuter les suites avant déploiement.

---

## 🤖 Auteur

**Geoffrey ROUVEL**  
Étudiant à l’IPSSI | Administrateur Systèmes & Réseaux  
GitHub : [@RgGeolll](https://github.com/RgGeolll)

---

## 🤖 Collaborateur

**Xavier ROCHER**  
Étudiant à l’IPSSI | Administrateur Systèmes & Réseaux  
GitHub : [@Xavier-ROCHER](https://github.com/Xavier-ROCHER)

**Ludovic MANGENOT**  
Étudiant à l’IPSSI | Administrateur Systèmes & Réseaux

---

🎓 Projet réalisé dans le cadre du module **AMAZON AWS : CI / CD ET AUTOMATISATION DES TACHES AVEC ANSIBLE ET JENKINS
** – Mastère Cybersécurité & Cloudcomputing.
