# BT Wholesale AEM Headless Project

**Content as a Service Platform for B2B Wholesale**

This Adobe Experience Manager as a Cloud Service (AEMaaCS) project is configured for **headless content delivery**, decoupling content management from presentation layers. The platform enables seamless content delivery across multiple digital channels via APIs.

## 🎯 Project Overview

BT Wholesale AEM provides a centralized content management platform that:
- ✅ Manages structured B2B content with Content Fragments
- ✅ Delivers content via GraphQL and REST APIs
- ✅ Scales content consumption across web, mobile, and third-party applications
- ✅ Maintains editorial control with workflow governance
- ✅ Supports multi-language and multi-region content strategies

## 🏗️ Architecture

### Headless CMS Structure

```
btwholesale/
├── core/                         # Java bundles & OSGi services
├── ui.apps/                      # Components, templates, clientlibs
├── ui.apps.structure/            # App structure definition
├── ui.config/                    # Global OSGi configurations
├── ui.content/                   # Content packages
├── ui.frontend/                  # Webpack frontend build pipeline
├── dispatcher/                   # Caching layer & request handling
├── it.tests/                     # Integration tests
├── ui.tests/                     # UI/E2E tests (Cypress)
└── all/                          # Aggregated deployment package
```

### Key Components

| Module | Purpose |
|--------|---------|
| **core** | Backend logic, APIs, services, event handlers |
| **ui.apps** | Components, templates, Content Fragment Models |
| **ui.frontend** | Webpack, TypeScript, SCSS compilation |
| **dispatcher** | CDN-friendly caching, request routing, security |
| **ui.config** | Environment-specific OSGi configurations |

## 📡 Content as a Service (CaaS)

### GraphQL API Delivery

Access structured content via GraphQL:

```graphql
{
  productList(limit: 10, offset: 0) {
    items {
      _path
      title
      description
      price
      category
      availability
      createdAt
    }
  }
}
```

**Endpoint**: `https://<instance>.aemcs.adobe.io/api/graphql/execute.json`

### REST API Endpoints

```
/api/assets                  # Digital asset delivery
/api/sites                   # Page and site content  
/api/content                 # Content fragments
/api/variations              # Content variations
```

## 🚀 Getting Started

### Prerequisites
- Java 11+
- Maven 3.6+
- Node.js 14+ (for frontend)
- AEMaaCS or AEM SDK environment

### Installation & Build

1. **Clone Repository**
   ```bash
   git clone https://github.com/mpanttech/btwholesale-aem.git
   cd btwholesale
   ```

2. **Build All Modules**
   ```bash
   mvn clean install
   ```

3. **Deploy to Local AEM SDK (Author)**
   ```bash
   mvn clean install -PautoInstallSinglePackage
   ```

4. **Deploy to Publish Instance**
   ```bash
   mvn clean install -PautoInstallSinglePackagePublish
   ```

5. **Deploy Specific Module** (e.g., ui.apps)
   ```bash
   cd ui.apps
   mvn clean install -PautoInstallPackage
   ```

### Frontend Development

```bash
cd ui.frontend
npm install
npm run build              # Production build
npm run build:scss         # Compile styles
npm run webpack:watch      # Watch mode development
```

## 📦 Module Details

### [core/](core/README.md)
Java bundle containing:
- GraphQL/REST API services
- OSGi services & configurations
- Workflow handlers
- Event listeners & schedulers
- Request filters & servlets
- Business logic components

### [ui.apps/](ui.apps/README.md)
Front-end resources:
- Content Fragment Models (CFM)
- Experience Fragments
- Reusable React/HTL components
- Client libraries (JS/CSS)
- Templates & policies

### [ui.content/](ui.content/README.md)
Sample content using the components from ui.apps

### [ui.frontend/](ui.frontend/README.md)
Dedicated frontend build:
- Webpack configuration (dev/prod)
- TypeScript/SCSS compilation
- Asset optimization
- Client library management

### [dispatcher/](dispatcher/README.md)
Caching and request handling:
- Cache invalidation rules
- Request routing & rewriting
- Security headers
- Performance optimization
- CDN integration

### [it.tests/](it.tests/README.md)
Integration tests validating backend functionality

### [ui.tests/](ui.tests/README.md)
Cypress-based UI and E2E tests

## 📋 Content Fragment Models

Define structured content with:
- Typed fields (text, number, date, reference)
- Multi-language support
- Field validation rules
- Optional/required constraints
- Related content associations

## 🔗 API Integration Example

### JavaScript/React

```javascript
async function fetchProducts() {
  const query = `{
    productList(limit: 20) {
      items {
        _path
        title
        description
        price
        category
      }
    }
  }`;

  const response = await fetch('/api/graphql/execute.json', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query })
  });

  const { data } = await response.json();
  return data.productList.items;
}
```

## 🚢 Deployment Pipeline

```
Git Commit → Cloud Manager Build → Deploy to Staging → Deploy to Production
```

### Cloud Manager

1. Commit changes to Git repository
2. Pipeline executes in Cloud Manager
3. Build & test phases validate
4. Deploy to staging environment
5. Approval & deploy to production

## ⚙️ Configuration

### OSGi Configurations

Environment-specific settings in:
- `ui.config/src/main/content/jcr_root/apps/<app-id>/osgiconfig/`

### Environment Variables

Set in Cloud Manager:
```
JAVA_TOOL_OPTIONS="-Xmx2048m -Xms512m"
GIT_REPO_URL=https://github.com/mpanttech/btwholesale-aem.git
```

## 🔒 Security

- ✅ CORS configuration for controlled API access
- ✅ Token-based API authentication
- ✅ Content versioning & audit trails
- ✅ SSL/TLS encryption
- ✅ Role-based access control (RBAC)
- ✅ DDoS protection via CDN

## 📊 Performance Optimization

- **Dispatcher Caching**: 80%+ cache hit rate target
- **GraphQL Optimization**: Query batching, field selection
- **Asset Optimization**: Automated image compression
- **Frontend Bundling**: Minified & tree-shaken output
- **API Response Time**: Target < 200ms

## 🧪 Testing

### Build & Test

```bash
# Run all tests
mvn clean install

# Run specific test module
mvn test -f it.tests/pom.xml

# Run UI tests
cd ui.tests && npm test
```

## 📚 Documentation

- [AEM Cloud Service Docs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/)
- [Headless CMS Guide](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/getting-started.html)
- [GraphQL API Reference](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/overview.html)
- [Content Fragments Guide](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/content-fragments/overview.html)

## 🤝 Contributing

1. Create feature branch: `git checkout -b feature/description`
2. Make changes and test locally
3. Commit with clear messages: `git commit -m "Add feature description"`
4. Push changes: `git push origin feature/description`
5. Create Pull Request
6. Pass CI/CD validation and code review

## 🆘 Troubleshooting

### Content Not Appearing in API
- Verify content is published
- Check replication queue status
- Validate Content Fragment Model configuration
- Review GraphQL query syntax

### Slow API Responses
- Monitor query complexity
- Check dispatcher cache hit rate
- Review database query performance
- Enable query caching

### Deployment Failures
- Review Cloud Manager pipeline logs
- Verify environment variables
- Validate OSGi configurations
- Check Java/Maven versions

## 📄 License

See LICENSE file for details

## 👥 Support

For issues or questions, please:
1. Check existing documentation and READMEs in each module
2. Review Cloud Manager logs
3. Contact the development team
4. Open an issue in the GitHub repository

There are three levels of testing contained in the project:

### Unit tests

This show-cases classic unit testing of the code contained in the bundle. To
test, execute:

    mvn clean test

### Integration tests

This allows running integration tests that exercise the capabilities of AEM via
HTTP calls to its API. To run the integration tests, run:

    mvn clean verify -Plocal

Test classes must be saved in the `src/main/java` directory (or any of its
subdirectories), and must be contained in files matching the pattern `*IT.java`.

The configuration provides sensible defaults for a typical local installation of
AEM. If you want to point the integration tests to different AEM author and
publish instances, you can use the following system properties via Maven's `-D`
flag.

| Property              | Description                                         | Default value           |
|-----------------------|-----------------------------------------------------|-------------------------|
| `it.author.url`       | URL of the author instance                          | `http://localhost:4502` |
| `it.author.user`      | Admin user for the author instance                  | `admin`                 |
| `it.author.password`  | Password of the admin user for the author instance  | `admin`                 |
| `it.publish.url`      | URL of the publish instance                         | `http://localhost:4503` |
| `it.publish.user`     | Admin user for the publish instance                 | `admin`                 |
| `it.publish.password` | Password of the admin user for the publish instance | `admin`                 |

The integration tests in this archetype use the [AEM Testing
Clients](https://github.com/adobe/aem-testing-clients) and showcase some
recommended [best
practices](https://github.com/adobe/aem-testing-clients/wiki/Best-practices) to
be put in use when writing integration tests for AEM.

## Static Analysis

The `analyse` module performs static analysis on the project for deploying into AEMaaCS. It is automatically
run when executing

    mvn clean install

from the project root directory. Additional information about this analysis and how to further configure it
can be found here https://github.com/adobe/aemanalyser-maven-plugin

### UI tests

They will test the UI layer of your AEM application using Cypress framework.

Check README file in `ui.tests` module for more details.

Examples of UI tests in different frameworks can be found here: https://github.com/adobe/aem-test-samples

## ClientLibs

The frontend module is made available using an [AEM ClientLib](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html). When executing the NPM build script, the app is built and the [`aem-clientlib-generator`](https://github.com/wcm-io-frontend/aem-clientlib-generator) package takes the resulting build output and transforms it into such a ClientLib.

A ClientLib will consist of the following files and directories:

- `css/`: CSS files which can be requested in the HTML
- `css.txt` (tells AEM the order and names of files in `css/` so they can be merged)
- `js/`: JavaScript files which can be requested in the HTML
- `js.txt` (tells AEM the order and names of files in `js/` so they can be merged
- `resources/`: Source maps, non-entrypoint code chunks (resulting from code splitting), static assets (e.g. icons), etc.

## Maven settings

The project comes with the auto-public repository configured. To setup the repository in your Maven settings, refer to:

    http://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html
