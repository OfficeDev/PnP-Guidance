# Proven Practices for SharePoint Online Portals - Custom Navigation

How to build a well-performing custom navigation system in SharePoint Online.

_**Applies to:** SharePoint Online_

## Rationale for a Custom Navigation Solution
<a name="bk_rationale"> </a>

There are a number of reasons why a Portal Architect might decide to build a custom navigation solution.  This generally happens when it is determined that the available Out-of-Box (OOB) server-side navigation controls cannot be configured to meet the needs of the proposed Portal Design. Typical examples follow:

- The OOB control, and its Management UX, does not support a Responsive UI Design
- The OOB control does not exhibit the required behaviors (flyout/hover, mega menu, rich media, lazy-load, etc.)
- The OOB control does not support the desired navigation hierarchy attributes (headers, grouping, depth, link limit, etc.) 
- The OOB control does not support the desired navigation link attributes (thumbnail, image link, publishing start/end, emphasis, etc.) 
- The OOB control is not, or is no longer, available (footer, breadcrumb, etc.)
- The OOB control does not integrate with custom/legacy navigation data stores
- The OOB Management UX is inconsistent across navigation controls and is not user-friendly 

## Typical Navigation Controls
<a name="bk_navControls"> </a>

A custom navigation solution typically includes one or more of the following types of controls:

- Global Navigation
- Current Navigation (i.e., Left-Hand)
- Breadcrumb Navigation
- Footer Navigation
- Site Map
- Useful Links (Governed or Personal)

## Logical Architecture of a Custom Navigation Solution
<a name="bk_logicalArchitecure"> </a>
The following diagram shows the logical architecture of a custom navigation solution:

[**TODO: UPDATE THIS IMAGE**]

![](\media\logicalNavSoln.png)

The following sections describe the major components of the logical architecture:

### Display Control
This is a custom client-side JavaScript display control that resides on the page. When the page loads, the control queries the Navigation Store, processes the navigation data response, and renders the navigation component (presentation, information, and behavior). 

The display control can be added to the static definition of the page at design time (via a master page (with caveats), page layout, or web part) or it can be added to the dynamic state of the page at run-time (via the JavaScript Embedding technique).

The display control leverages the Client-Side Data Access Framework to optimize page performance.

The display control may optionally provide a Settings link for its Navigation Management Page, which provides a user interface to manage the configuration of the navigation control in a user-friendly manner.

### Navigation Store
The Navigation Store persists the configuration of the navigation control. 

The most-commonly used custom Navigation Store, a custom SharePoint list, strikes a balance between extensibility, manageability, and performance. The list schema can be easily extended with custom Content Types and Site Columns that represent navigation headers/groups and navigation links. Crawled Properties for these Site Columns can be mapped to Managed Properties within SharePoint Search. The navigation data is easily managed via the familiar OOB List Management pages. The navigation data can be accessed remotely via the SharePoint Search REST API.

The simplest and best-performing custom Navigation Store is a JavaScript resource file (e.g., nav.js) that declares a component-specific configuration variable (e.g., footerNav) that is initialized with a JSON string. The browser requests the file and caches it for subsequent use.  The configuration data is ready for use once it loads into the JavaScript run-time environment. The primary trade-off with this approach concerns the Management User Interface: at a minimum, an admin must manually edit a JSON string in a JavaScript file. A custom User Interface would be required to hide the Store from the admin and make things a bit more friendly.

At the other end of the spectrum of custom Navigation Stores is the custom database. This option provides the ultimate in flexibility, but every aspect of this option requires custom development as well as hosting for the database, its custom API, and its Management interface. 

Other [TBD]:

- OOB Site Navigation Managed (MMS)
- OOB Site Navigation Structured
- Managed Metadata Service (MMS) Termset

### Navigation Management Page
The Navigation Management Page provides a user interface to manage the configuration of the navigation control in a user-friendly manner.  The page can be accessed directly, as well as from an optional link present on the navigation control (e.g., a Settings link). The page uses the appropriate Navigation Store APIs for the chosen Navigation Store to manage the configuration of the navigation control.

In many cases, the default OOB Navigation Management Page associated with the chosen Navigation Store should suffice. When a default page is not available, you must obviously develop a custom page. When deciding if the existing default page is acceptable or not, be sure to consider the total cost of the page (design, development, hosting, and user training).

As a rule of thumb, pursue custom Management Pages only when a default option does not exist, when the page must support a Responsive UI, or when the page is meant to be consumed via the front-end user view of the Portal (as opposed to the back-end admin view).  

### Navigation Store API
The Navigation Store API provides a programmatic interface to manage the configuration of the navigation control in a consistent, secure manner.

Provide a Web API for any custom services that you develop and deploy. Use Public DNS for name resolution, require SSL, use a public certificate authority for the SSL certificate, enable anonymous access, secure the API with Azure AD, and implement support for Cross-Origin Resource Support (CORS).

For .Net client environments

- target SharePoint via the SharePoint Client-Side Object Model (CSOM) 
- target your custom Web APIs via REST
- target third-party services via REST (or SOAP if necessary)

For Browser client environments

- target SharePoint via the SharePoint REST APIs
	- Use the Cross-Domain Library if you target a different site collection
- target your custom Web APIs via REST; use Implicit OAuth
- target third-party services via REST (or SOAP if necessary)

### Client-Side Data Access Layer
The Client-Side Data Access Layer is a custom client-side JavaScript framework made available to all custom client-side display controls.  It supports intelligent data loading patterns, provides data caching functionality to minimize client-to-server request traffic, and improves perceived page performance.

Please refer to the Portal Performance article for more information on the Client-Side Data Access Layer

- [Portal performance guidance](portal-performance.md)

## Additional Resources
<a name="bk_additionalResources"> </a>

add links