# Assignment 3: Microsoft Deployment Toolkit

## Assignment

You wish to automate the rollout of software on the workstations (clients) in a typical SME office network with a Windows environment. You also wish to automate the company's web server. Within a Windows environment you can do this efficiently with "Microsoft Deployment Toolkit (MDT)".

Use Virtualbox to perform these installations. Do not use Vagrant. This is going to give you a fair amount of trouble setting up. Use the toolkit MDT, and where necessary, use PowerShell to do certain roll-outs of the server and possibly also the clients. Each installation must be done via PXEboot.

Install the following setup. All installations are with Windows Server 2019, and Windows 10 for the clients/workstations. Perform all updates on the different machines before proceeding to configuration.

- 1 Domain Controller
    - domain name is `CoronaPrj.local`
    - hostname: `GroupxxDC` with `xx` your group number
- 1 Member Server, member of the above domain.
    - host name: `GroupxxAut`
- Client systems (added to the above domain)
    - hostnames `GroupxxCly` with `y` an ascending number

Install the following items within the domain:

- Roles AD - DNS - DHCP (on later DC)
- WSUS roles on later Member Server
- Software: MDT on later Member Server

**WSUS Windows Server Update Services:**

- Provide system updates for Clients
    - For client updates provide the Dutch updates and the English update
- System updates for Servers
    - For server updates, you only provide the English updates

**MDT - Microsoft Deployment Toolkit:**

Use MDT, located at <https://www.microsoft.com/en-us/download/details.aspx?id=54259>, to automatically install the following software packages on the clients:

Client Image:

- Windows 10
    - Clean installation for new systems
- Adobe Reader
    - Must be installed on new and existing devices that do not yet have the software
    - For new installations you can integrate it into your image of the Windows Client
Java
    - Must be installed on new and existing systems.
    - For new installations you can integrate it into your image of the Windows Client
- Office suite (eg LibreOffice)

Server Image:

Make sure you can install a Windows Server 2019 automatically using MDT. Provide different task sequences in your MDT, which means that you can deploy different types of servers:

- Task Sequence 1:
    - Windows 2019 clean install
- Task Sequence 2:
    - Windows 2019 with the following components
        - ASP.NET
        - IIS
- Task Sequence 3:
    - Microsoft SQL Server.

More info available at this link: <https://docs.microsoft.com/en-be/configmgr/mdt/index>

## Deliverables

- Demo during the progress meetings of the proof-of-concept
    - Automatic installation Windows Server and Windows 10
- On Github:
    - Specifications
    - All background information that you have collected to get started with the assignment
    - Detailed technical manuals addressed to other team members on installation procedures and the scripts used
    - Test plans and test reports
