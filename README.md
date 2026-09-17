# SERP Hawk CRM V2

AI-Powered CRM for SEO Agencies | Next.js + FastAPI + PostgreSQL + OpenAI

## Overview

SERP Hawk CRM V2 is a comprehensive customer relationship management system designed specifically for SEO agencies and digital marketing firms. It manages the entire client lifecycle from cold outreach to project delivery, billing, and SEO monitoring.

### Key Features

* **Role-Based Access**: Admin, Employee, Intern, Client roles with appropriate permissions
* **AI Email Agent**: Automated company research and personalized email generation
* **Real-Time Messaging**: WebSocket-based chat system
* **Service Management**: Catalog, quotes, invoicing, and billing
* **SEO Tools**: Keyword rankings, competitor analysis, SEO audits
* **Document Management**: File uploads, OCR for business cards
* **Reporting**: PDF exports, monitoring dashboards

## Tech Stack

* **Frontend**: Next.js 16, React 19, TypeScript, Tailwind CSS 4, Framer Motion
* **Backend**: FastAPI, Python, SQLModel ORM, Uvicorn with WebSocket
* **Database**: PostgreSQL
* **AI**: OpenAI GPT-4o-mini, Google Gemini (OCR)
* **Integrations**: Outlook SMTP/IMAP, Webhooks, ReportLab PDFs

## AWS Deployment

The application is deployed on an AWS EC2 Ubuntu server.

### AWS Services

* **Amazon EC2** – Hosts the Next.js frontend, FastAPI backend, and PostgreSQL database.
* **Nginx** – Reverse proxy that routes web and API traffic.
* **PM2** – Process manager for the Next.js frontend and FastAPI backend.
* **PostgreSQL** – Stores application and CRM data.
* **Ubuntu** – Operating system running on the EC2 instance.

### Production URLs

* **Application**: http://13.51.165.2
* **API Documentation**: http://13.51.165.2/api/docs

### Production Architecture

```text
                         Internet
                            |
                            v
                     AWS EC2 Instance
                            |
                         Nginx :80
                            |
                +-----------+-----------+
                |                       |
                v                       v
         Next.js :3000            FastAPI :8000
                |                       |
                |                       v
                |                  PostgreSQL
                |
                v
            CRM Web UI
```

The FastAPI service is bound to `127.0.0.1:8000` and is accessed through Nginx. PostgreSQL is also kept on the server and is not exposed publicly.

### Request Routing

* `/` → Next.js frontend
* `/api/*` → FastAPI backend
* `/api/docs` → FastAPI Swagger documentation

### Deployment Process

1. Launch an Ubuntu EC2 instance on AWS.
2. Configure the EC2 security group for HTTP and secure SSH access.
3. Clone the GitHub repository.
4. Create the Python virtual environment.
5. Install backend dependencies.
6. Configure PostgreSQL and initialise the database.
7. Configure application environment variables.
8. Install frontend dependencies.
9. Create the Next.js production build.
10. Configure Nginx as a reverse proxy.
11. Run the application services using PM2.
12. Configure PM2 to start services automatically after an EC2 reboot.
13. Verify the frontend, authentication, API and database connectivity.

## Deployment Configuration

### Backend

Create a Python virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Initialise the database:

```bash
python create_tables.py
python seed_db.py
```

The FastAPI backend runs internally on:

```text
127.0.0.1:8000
```

### Frontend

Navigate to the frontend directory:

```bash
cd frontend
npm install
npm run build
```

The production frontend runs on:

```text
127.0.0.1:3000
```

### Nginx

Nginx provides the public HTTP entry point and routes requests internally:

```text
/api/*  →  FastAPI :8000
/*      →  Next.js :3000
```

### PM2

PM2 is used to keep the application services running and to restore the saved process list after an EC2 reboot.

Check the processes with:

```bash
pm2 status
```

Save the process list with:

```bash
pm2 save
```

## Environment Variables

Create a `.env` file in the root directory:

```env
DATABASE_URL=postgresql://user:password@host:port/database

OPENAI_API_KEY=your_openai_api_key

GEMINI_API_KEY=your_gemini_api_key

SECRET_KEY=your_secret_key

SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your_email@gmail.com
SMTP_PASSWORD=your_app_password
```

For the frontend, configure:

```env
NEXT_PUBLIC_API_BASE_URL=/api
```

> Never commit real passwords, API keys, database credentials, SMTP credentials, or other secrets to the repository.

## Local Development

### Backend

1. Create virtual environment:

```bash
python -m venv .venv
```

2. Activate:

```bash
source .venv/bin/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run database setup:

```bash
python create_tables.py
```

5. Start the server:

```bash
uvicorn main:app --reload
```

Local backend API:

```text
http://127.0.0.1:8000
```

Local Swagger:

```text
http://127.0.0.1:8000/docs
```

### Frontend

1. Navigate to frontend:

```bash
cd frontend
```

2. Install dependencies:

```bash
npm install
```

3. Start development server:

```bash
npm run dev
```

## Database Setup

1. Create a PostgreSQL database.
2. Configure the `DATABASE_URL` environment variable.
3. Run:

```bash
python create_tables.py
```

4. Seed initial data if required:

```bash
python seed_db.py
```

## How to Add New Features

### Backend (FastAPI)

1. **Add Database Models**

   * Edit `database.py` to add new SQLModel classes.
   * Run `python create_tables.py` to create tables.

2. **Create API Endpoints**

   * Add routes in `main.py` or create new modules.
   * Follow RESTful conventions.
   * Add appropriate authentication and authorization.

3. **Add Business Logic**

   * Create functions in appropriate modules under `modules/`.
   * Use dependency injection for database sessions.

4. **Update Dependencies**

   * Add dependencies to `requirements.txt`.
   * Test with `pip install -r requirements.txt`.

### Frontend (Next.js)

1. **Create New Pages**

   * Add pages under `frontend/src/app/`.
   * Use TypeScript for type safety.

2. **Add Components**

   * Create reusable components under `frontend/src/components/`.
   * Follow existing project patterns.

3. **API Integration**

   * Use the existing API utilities under `frontend/src/lib/`.
   * Add new API calls as required.

4. **Styling**

   * Use Tailwind CSS classes.
   * Follow the existing design system.

### General Steps

1. Plan the feature and database changes.
2. Implement backend API endpoints.
3. Update frontend to consume the new APIs.
4. Add proper error handling and validation.
5. Test thoroughly.
6. Update documentation.

### Example: Adding a New Entity

1. Define the model in `database.py`.
2. Create CRUD endpoints in `main.py`.
3. Create frontend pages for list/view/edit.
4. Add navigation links.
5. Test the full flow.

## API Documentation

### Production

Swagger UI:

```text
http://13.51.165.2/api/docs
```

### Local Development

Swagger UI:

```text
http://127.0.0.1:8000/docs
```

ReDoc:

```text
http://127.0.0.1:8000/redoc
```

## Production Verification

The AWS deployment was verified for:

* Frontend availability
* User authentication/login
* Backend API availability
* PostgreSQL connectivity
* Swagger API documentation
* Nginx reverse proxy routing
* PM2 process management
* PM2 startup persistence after EC2 reboot
* Same-origin API routing through `/api`

## Contributing

1. Fork the repository.
2. Create a feature branch.
3. Make your changes.
4. Test thoroughly.
5. Submit a pull request.

## License

This project is proprietary. All rights reserved.
