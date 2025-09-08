
# Jenkins con JCasC — Laboratorio reproducible (Docker Compose)

Este repo contiene un laboratorio listo para levantar **Jenkins** configurado con **Configuration as Code (JCasC)**, instalar **plugins**, crear **una carpeta**, **un pipeline** apuntando a **GitHub**, y **un secreto** para usar en el pipeline. Además incluye un `Jenkinsfile` de ejemplo.

---

## 📦 Estructura

```
jenkins-lab/
├─ README.md
├─ Jenkinsfile
├─ docker-compose.yml
├─ .env.example
└─ jenkins/
   ├─ Dockerfile
   ├─ plugins.txt
   └─ casc.yaml
```

---

## ✅ Requisitos

- Docker y Docker Compose instalados.
- (Opcional para repos privados) Un **GitHub Personal Access Token (PAT)** con scope `repo`.
- Un repositorio en GitHub donde **subir este código**. Luego actualizas la variable `GITHUB_REPO` para que apunte a tu repo.

> Si tu repo es **público**, el clon se hace sin credenciales. El laboratorio igualmente crea credenciales por si las necesitas.

---

## 🔧 Variables de entorno

Copia `.env.example` a `.env` y ajusta los valores:

```bash
cp .env.example .env
```

**`.env`** (explicación de cada variable):
- `JENKINS_ADMIN_ID`, `JENKINS_ADMIN_PASSWORD`: credenciales del admin de Jenkins.
- `GITHUB_USER`, `GITHUB_TOKEN`: usuario y PAT de GitHub (necesario solo para repos privados o si quieres usar APIs autenticadas).
- `GITHUB_REPO`: URL HTTPS de tu repo (por ejemplo, el repo donde subas este código).
- `GITHUB_BRANCH`: rama con el `Jenkinsfile` (por defecto `main`).

---

## ▶️ Levantar Jenkins

Desde la raíz del proyecto:

```bash
docker compose up -d --build
```

Abre <http://localhost:8080> y entra con el usuario y password del `.env`.

Deberías ver:
- Mensaje “**Jenkins configurado por JCasC — laboratorio**”
- Carpeta **`laboratorio`**
- Job **`laboratorio/ci-demo`** (lee el `Jenkinsfile` de tu repo)

> Lánzalo manualmente la primera vez o espera al polling (cada 2 minutos en este ejemplo).

---

## 🧠 ¿Qué crea JCasC?

- Un usuario admin local.
- **Credenciales**:
  - `github-https` (usuario + PAT) para clonar por HTTPS.
  - `github-token` como **Secret Text** para usarlo en pipelines.
- Una **carpeta** `laboratorio`.
- Un **pipeline** `laboratorio/ci-demo` que clona `${GITHUB_REPO}` y usa el `Jenkinsfile` del repo.

---

## 🧪 Jenkinsfile de ejemplo

Este repo incluye un `Jenkinsfile` funcional. Si subes este mismo proyecto a GitHub y apuntas `GITHUB_REPO` a tu repo, el job lo usará tal cual.

---

## 🔄 Recargar configuración

Cambia `.env` o `jenkins/casc.yaml` y luego:
- Reinicia el contenedor, o
- En Jenkins: **Manage Jenkins → Configuration as Code → Reload**.

---

## 🧹 Limpieza

```bash
docker compose down -v
```

`-v` borra el volumen `jenkins_home` (plugins, jobs, etc.).

---

## 🩺 Troubleshooting

- **El job no aparece**: confirma que `CASC_JENKINS_CONFIG` en `docker-compose.yml` apunta a `jenkins/casc.yaml` y que `job-dsl` está instalado (lo instala `plugins.txt`).
- **Clonado fallido**: si el repo es privado, revisa `GITHUB_USER`/`GITHUB_TOKEN` y que el PAT tenga scope `repo`. Asegura que `GITHUB_REPO` y `GITHUB_BRANCH` sean correctos.
- **Falta `git`**: lo instalamos en `jenkins/Dockerfile`. Si no aparece, confirma que reconstruiste la imagen con `--build`.

---

## ✍️ Créditos

Hecho para un laboratorio mínimo y reproducible con **Jenkins LTS + JCasC**.
# jenkins_casc
