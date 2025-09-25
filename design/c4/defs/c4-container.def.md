# Example of a C4 Container Diagram in Mermaid

## C4 Container Diagram definition

Once you understand how your system fits in to the overall IT environment, a really useful next step is to zoom-in to the system boundary with a Container diagram.
A “container” is something like a server-side web application, single-page application, desktop application, mobile app, database schema, file system, etc.
Essentially, a container is a separately runnable/deployable unit (e.g. a separate process space) that executes code or stores data.

The Container diagram shows the high-level shape of the software architecture and how responsibilities are distributed across it.
It also shows the major technology choices and how the containers communicate with one another.
It's a simple, high-level technology focussed diagram that is useful for software developers and support/operations staff alike.

## Mermaid C4 Components for Container Diagrams

### People Components
- `Person(id, "Name", "Description")` - Internal user/actor who interacts with containers
- `Person_Ext(id, "Name", "Description")` - External user/actor outside your organization

### System Components (External)
- `System_Ext(id, "Name", "Description")` - External system that your containers interact with
- `SystemDb_Ext(id, "Name", "Description")` - External database system
- `SystemQueue_Ext(id, "Name", "Description")` - External message queue system

### Container Components
- `Container(id, "Name", "Technology", "Description")` - A deployable unit within your system
- `Container_Ext(id, "Name", "Technology", "Description")` - External container outside your system boundary
- `ContainerDb(id, "Name", "Technology", "Description")` - Database container within your system
- `ContainerDb_Ext(id, "Name", "Technology", "Description")` - External database container
- `ContainerQueue(id, "Name", "Technology", "Description")` - Message queue container within your system
- `ContainerQueue_Ext(id, "Name", "Technology", "Description")` - External message queue container

### Boundary Components
- `Container_Boundary(id, "Name")` - Groups containers within your system boundary
- `Enterprise_Boundary(id, "Name")` - Groups elements within your enterprise/organization
- `System_Boundary(id, "Name")` - Groups related systems or containers together
- `Boundary(id, "Name", "Type")` - Generic boundary for grouping elements

### Relationship Components
- `Rel(from, to, "Label")` - Unidirectional relationship with label
- `Rel(from, to, "Label", "Technology")` - Unidirectional relationship with label and technology
- `Rel_Back(from, to, "Label")` - Reverse direction relationship
- `Rel_Back(from, to, "Label", "Technology")` - Reverse direction relationship with technology
- `BiRel(from, to, "Label")` - Bidirectional relationship with label

### Diagram Structure
- `C4Container` - Declares this as a C4 Container diagram
- `title "Diagram Title"` - Sets the diagram title

## C4 Container Diagram Mermaid example

```mermaid
    C4Container
    title Container diagram for Internet Banking System

    System_Ext(email_system, "E-Mail System", "The internal Microsoft Exchange system", $tags="v1.0")
    Person(customer, Customer, "A customer of the bank, with personal bank accounts", $tags="v1.0")

    Container_Boundary(c1, "Internet Banking") {
        Container(spa, "Single-Page App", "JavaScript, Angular", "Provides all the Internet banking functionality to customers via their web browser")
        Container_Ext(mobile_app, "Mobile App", "C#, Xamarin", "Provides a limited subset of the Internet banking functionality to customers via their mobile device")
        Container(web_app, "Web Application", "Java, Spring MVC", "Delivers the static content and the Internet banking SPA")
        ContainerDb(database, "Database", "SQL Database", "Stores user registration information, hashed auth credentials, access logs, etc.")
        ContainerDb_Ext(backend_api, "API Application", "Java, Docker Container", "Provides Internet banking functionality via API")

    }

    System_Ext(banking_system, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.")

    Rel(customer, web_app, "Uses", "HTTPS")
    UpdateRelStyle(customer, web_app, $offsetY="60", $offsetX="90")
    Rel(customer, spa, "Uses", "HTTPS")
    UpdateRelStyle(customer, spa, $offsetY="-40")
    Rel(customer, mobile_app, "Uses")
    UpdateRelStyle(customer, mobile_app, $offsetY="-30")

    Rel(web_app, spa, "Delivers")
    UpdateRelStyle(web_app, spa, $offsetX="130")
    Rel(spa, backend_api, "Uses", "async, JSON/HTTPS")
    Rel(mobile_app, backend_api, "Uses", "async, JSON/HTTPS")
    Rel_Back(database, backend_api, "Reads from and writes to", "sync, JDBC")

    Rel(email_system, customer, "Sends e-mails to")
    UpdateRelStyle(email_system, customer, $offsetX="-45")
    Rel(backend_api, email_system, "Sends e-mails using", "sync, SMTP")
    UpdateRelStyle(backend_api, email_system, $offsetY="-60")
    Rel(backend_api, banking_system, "Uses", "sync/async, XML/HTTPS")
    UpdateRelStyle(backend_api, banking_system, $offsetY="-50", $offsetX="-140")
```
