# Labware Tracker Flask API

This project is a containerised Flask application designed to store and retrieve labware information for robotic workflows in biomedical laboratories. It supports **GET** and **POST** requests and is deployed via a **CI/CD pipeline** using **GitHub Actions** and **Google Cloud Platform (GCP)**.

---

## Features

* `GET /labware`
  Retrieves all labware records stored in memory.

* `POST /labware`
  Adds a new labware record. Requires a JSON payload with `"name"` and `"type"`.

---

## File Structure

```
├── app.py                 # Main Flask API
├── Dockerfile             # Containerisation script
├── requirements.txt       # Python dependencies
├── .github/workflows/     # CI/CD workflows
└── README.md              # Project documentation
```

---

## Run Locally

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/Labware-Tracker.git
   cd Labware-Tracker
   ```

2. Install dependencies:

   ```bash
   pip install flask
   ```

3. Run the Flask app:

   ```bash
   python app.py
   ```

---

## Containerisation with Docker

To containerise the app:

1. Create a `requirements.txt` with:

   ```
   flask
   ```

2. Create the Dockerfile:

   ```dockerfile
   FROM python:3.9-slim

   WORKDIR /app

   COPY requirements.txt .
   RUN pip install -r requirements.txt

   COPY . .

   CMD ["python", "app.py"]
   ```

3. Build and run the Docker container:

   ```bash
   docker build -t labware-api .
   docker run -d -p 5000:5000 labware-api
   ```

---

## CI/CD Deployment (GitHub + Google Cloud)

This project is automatically deployed to **Google Cloud Run** using a **GitHub Actions workflow**.
This allows us to:

* Build Docker image from your code.
* Pushe images to **Google Container Registry (GCR)**.
* Deploy the image to **Cloud Run**.

###  Required GitHub Secrets:


* `GCP_PROJECT_ID`
* `GCP_SA_KEY` (Base64-encoded service account key)
* `GCP_REGION` (e.g `us-central1`)
* `GCP_ZONE` (e.g `us-central1-a`)
* `GCP_BUCKET_NAME` (e.g `terraform-state-labwaretrackerapp`)

### Trigger:

* On every push to `main`, the workflow builds, pushes, and deploys the container.

---

## Sample API Usage

**POST new labware:**

```bash
curl -X POST http://localhost:5000/labware \
     -H "Content-Type: application/json" \
     -d '{"name": "Rack", "type": "96-well"}'
```

**GET labware list:**

```bash
curl http://localhost:5000/labware
```

---

## Summary

This Flask API project demonstrates:

* API development for real-world lab automation tasks.
* Docker-based containerisation.
* Fully automated deployment with GitHub Actions and GCP Cloud Run.
