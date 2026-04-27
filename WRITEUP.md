# Project Write-up - CMS Deployment in Azure

### Analyze, choose, and justify the appropriate resource option for deploying the app.

#### Analysis: VM vs. App Service
In this project, I evaluated two primary Azure hosting options: **Virtual Machines (VM)** and **Azure App Service**.

* **Costs:** * **VM:** Costs are fixed based on the instance size. However, the indirect costs are higher due to the manual labor required for patching, security updates, and server maintenance.
    * **App Service:** More cost-effective for web applications. The "Pay-as-you-go" model and reduced administrative overhead make it cheaper for small to medium CMS apps.
* **Scalability:** * **VM:** Scalability is manual or requires complex Virtual Machine Scale Sets (VMSS) configuration.
    * **App Service:** Offers built-in **Autoscaling**. It can automatically increase or decrease instances based on CPU or memory load with just a few clicks.
* **Availability:** * **VM:** High availability must be manually configured using Load Balancers and Availability Zones.
    * **App Service:** High availability is a core feature of the platform (PaaS). Microsoft guarantees 99.95% uptime without manual infrastructure setup.
* **Workflow:** * **VM:** The workflow is complex, requiring manual installation of Python, Gunicorn, Nginx, and SSL certificates.
    * **App Service:** Extremely streamlined. It integrates directly with GitHub for CI/CD. The Oryx build engine automatically detects the Flask environment and handles dependencies.

#### Chosen Solution: Azure App Service
I chose **Azure App Service** for deploying this CMS application.

#### Justification
The App Service is the superior choice for this Flask-based CMS because it is a **Platform-as-a-Service (PaaS)**. 
1. It allows me to focus entirely on the **application code** and the MSAL authentication logic rather than server management. 
2. The built-in **Logging Stream** (which I used to capture the successful admin login) is much easier to access and configure than setting up a centralized logging server for a VM. 
3. Security is handled by Azure, ensuring the Python runtime is always patched and the SSL termination is managed automatically.

---

### Assess app changes that would change your decision.

I would change my decision and opt for a **Virtual Machine (VM)** if the following requirements arose:

1.  **Custom OS Dependencies:** If the CMS required specific binary files, legacy drivers, or software that must be installed at the OS level and is not supported by the App Service sandbox.
2.  **Full OS Control:** If compliance or security regulations required deep access to the kernel or the ability to run specific security agents that require root access to the underlying OS.
3.  **Non-HTTP Protocols:** If the application needed to communicate over protocols other than HTTP/HTTPS or required a wide range of open TCP/UDP ports for specialized services.
4.  **Heavy Persistent Storage:** While App Service supports Azure Files, a VM would be better if the application required high-performance, local, raw block storage (Disk) for extremely data-intensive operations.
