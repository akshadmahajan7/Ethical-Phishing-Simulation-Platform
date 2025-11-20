# 🎣 Ethical Phishing Simulation Platform ($\text{PhishWise}$)

---

## 🌟 Project Overview
$\text{PhishWise}$ is a web-based platform designed to execute and manage controlled, ethical phishing simulations for employee training and security awareness purposes. The objective is to safely test an organization's human factor susceptibility to social engineering, gather actionable data, and provide immediate, relevant education to users who interact with the simulated attacks. This platform is strictly intended for authorized, internal testing in a secure lab environment.

---

## ✨ Features and Technology Stack
| **Component** | **Technology** | **Description** |
| --- | --- | --- | 
| **Primary Framework** | $\text{Python}$ ($\text{Flask}$) | Backend routing, campaign management logic, and dashboard rendering. | 
| **Database** | $\text{SQLite}$ | Stores campaign definitions, user lists, and tracked metrics (clicks, inputs, timestamps). | 
| **Email Transport** | $\text{Sendmail}$ / $\text{Postfix}$ | Secure, self-hosted $\text{SMTP}$ integration for controlled, internal email dispatch. | 
| **Frontend** | $\text{HTML}$, $\text{CSS}$, $\text{Jinja2}$ | Used for designing realistic, customizable phishing email templates and the analytics dashboard. | 
| **Tracking Logic** | $\text{Flask}$ Blueprints & $\text{DB}$ Counters | Implements 1x1 pixel tracking for opens and unique, parameterized URLs for clicks/inputs.| 

---

## 🛡️ Core Simulation & Tracking Strategy
The system follows a defined lifecycle to ensure a controlled and measurable simulation:
### Phase 1: Campaign Configuration
* **Template Customization**: Operators define the simulation by selecting an email template (e.g., password reset, HR alert) and customizing the target landing page ($\text{HTML}/\text{CSS}$).
* **Target List**: A secure list of internal test users is loaded into the $\text{SQLite}$ database.
### Phase 2: Email Dispatch and Data Collection
* **Personalized Links**: Each email sent contains unique, personalized tracking links (e.g., http://phishwise/track?u=user\_id\&c=campaign\_id) embedded in the simulated phishing $\text{URL}$.
* **Open Tracking**: A $\text{1x1}$ invisible pixel is embedded. Loading this pixel registers an "Email Open" event in the database.
* **Click/Input Tracking**: Clicking the malicious link redirects the user to the simulated login page. Submitting credentials on this page registers a "Success" event and captures the simulated inputs.
### Phase 3: Analytics and Education
* **Real-Time Analytics**: The $\text{Flask}$ dashboard processes the collected $\text{SQLite}$ data to display key metrics: $\text{Open Rate}$, $\text{Click Rate}$, and $\text{Compromise Rate}$ (Success Rate).
* **Immediate Feedback**: Users who submit data are immediately redirected to a mandatory educational page, detailing the phishing indicators they missed and providing best practices.

---

## 📁 Project Structure
```
PhishWise/
├── app/
│   ├── blueprints/                      # Modular application components (Blueprints in Flask)
│   │   ├── auth/                        # User authentication (login, registration)
│   │   │   ├── __init__.py
│   │   │   └── routes.py
│   │   ├── campaign_management/         # Handling campaign creation, launch, and monitoring
│   │   │   ├── __init__.py
│   │   │   └── routes.py
│   │   ├── reporting/                   # Data processing and analytics reporting
│   │   │   ├── __init__.py
│   │   │   └── routes.py
│   │   └── templates/                   # Template CRUD/editing for emails/pages
│   │       ├── __init__.py
│   │       └── routes.py
│   ├── templates/
│   │   ├── base.html                        # Main layout template (header, footer, navigation)
│   │   ├── dashboard/                       # Main application views
│   │   │   ├── dashboard.html               # Main analytics summary
│   │   │   ├── campaign_list.html           # Table view of all campaigns
│   │   │   └── reports.html                 # Detailed report view
│   │   ├── emails/                          # Phishing email templates
│   │   │   ├── corporate_announcement.html
│   │   │   └── suspicious_login_alert.html
│   │   ├── landing_pages/                   # Fake login/input pages (the target)
│   │   │   ├── office365_login.html
│   │   │   └── hr_survey.html
│   │   └── education/                       # Remediation and training pages
│   │       ├── education_module_1.html
│   │       └── phishing_quiz.html
│   ├── static/                              # Frontend Assets
│   │   ├── css/
│   │   │   └── main.css                     # Core styles
│   │   ├── js/
│   │   │   └── charts.js                    # Logic for dashboard charts (e.g., using Chart.js)
│   │   ├── img/
│   │   │   └── logo.png
│   │   └── vendor/                          # Third-party libraries (Bootstrap, jQuery)
│   ├── models.py                            # Database Models (SQLAlchemy/Alembic setup for MySQL)
│   ├── utils.py                             # Helper functions (e.g., email sending logic)
│   └── __init__.py                          # Application factory (initialization of app, DB, Mail)
├── migrations/                              # Database migration scripts (e.g., using Alembic/Flask-Migrate)
│   ├── env.py
│   ├── script.py.mako
│   └── versions/  
├── config.py                                # Configuration variables (MySQL URI, Mail settings)
├── requirements.txt                         # Python dependency list
└── run.py                                   # Application entry point
```

---

## 🚀 Getting Started
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

###Prerequisites
* $\text{Python 3.8+}$$\text{pip}$ (Python package installer)A configured local $\text{SMTP}$ server (e.g., $\text{Postfix}$ running locally on port $\text{25}$).

### Installation
1. Clone the repository:
```bash
git clone https://github.com/your-username/PhishWise.git
cd PhishWise
```
2. Create and activate a virtual environment:
```bash
python3 -m venv venv
source venv/bin/activate 
```
4. Install dependencies:
```bash
pip install -r requirements.txt
```

### Running the Application
1. Initialize the database:
```bash
python src/db/init_db.py  # Custom script to create phishwise.db and tables
```
2. Configure SMTP settings (usually via environment variables or a config file).
3. Start the $\text{Flask}$ web server:
```bash
python app/main.py
```
Access the application in your browser at `http://127.0.0.1:5000/`.

---

## 🤝 Contributing
We highly value contributions to improve the security and usability of this training platform. Please fork the repository and submit a Pull Request.
**Contribution Points**
* SMTP Integration: Abstract $\text{smtp\_client.py}$ to support $\text{API}$ integrations (e.g., $\text{SendGrid}$ or $\text{Mailgun}$) for easier deployment in non-lab environments.
* Template Library: Develop and integrate a comprehensive library of high-quality, professional-looking phishing email and landing page templates.
* Scheduled Campaigns: Implement a $\text{Celery}$ or $\text{APScheduler}$ task to allow campaign scheduling and automatic stopping/reporting.
* Dashboard Enhancement: Integrate a graphing library (e.g., $\text{Chart.js}$) into the dashboard templates for richer, time-series analytics visualization.

---

## 📝 License
This project is open-source and available under the MIT License.You can find the full license text here: [LICENSE](LICENSE)
