# LoKart

### Redefining Local Commerce

LoKart is a hyperlocal commerce platform designed to connect **local shops, nearby customers, and local delivery** through a single digital ecosystem.

Instead of depending entirely on centralized dark stores, LoKart enables customers to discover products and services from nearby local businesses and allows those businesses to reach more customers within their neighborhood.

---

## About the Project

LoKart aims to bring local commerce online while keeping the connection between **customers and local businesses** at the center of the platform.

The platform is designed to support:

* Discovery of nearby local shops and products
* Online ordering
* Local delivery
* Customer and shop-owner accounts
* Shop management
* Order management
* Local business visibility
* A cinematic and modern user experience

The project includes both the **customer-facing experience** and the functionality required for **local businesses and administrators**.

---

## Documentation

The project contract and architecture system is fully documented in `/docs`:

- [Product Requirements Document (PRD)](docs/PRD.md)
- [Product Vision & Strategy](docs/PRODUCT-VISION.md)
- [Technical Stack Specification](docs/TECH-STACK.md)
- [System Architecture](docs/ARCHITECTURE.md)
- [Database Specification](docs/DATABASE.md)
- [API Specification](docs/API-SPEC.md)
- [Authentication & Authorization Specification](docs/AUTH-SPEC.md)
- [Feature Specifications](docs/FEATURE-SPEC.md)
- [UI/UX Specification](docs/UI-UX-SPEC.md)
- [Security Guidelines](docs/SECURITY.md)
- [Testing Strategy](docs/TESTING.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Phased Roadmap](docs/ROADMAP.md)
- [Architecture Decision Records (ADRs)](docs/ADR/001-frontend.md)
- [AI Change Logs](docs/ai-changes/README.md)

---

### Customer

* User registration and login
* Browse nearby shops
* Explore products and services
* Search and discover local businesses
* Add products to cart
* Place orders
* Track order status
* Local delivery support
* Order history
* Customer profile

### Local Shops

* Shop registration and login
* Shop profile management
* Add and manage products
* Manage product availability
* Receive customer orders
* Accept or manage orders
* View order information
* Manage shop information
* Reach customers within the local area

### Admin

* Admin authentication
* Manage users
* Manage local shops
* Monitor orders
* Manage platform data
* Monitor and maintain the overall LoKart ecosystem

---

## Development Demo Authentication

> [!WARNING]
> **These are development-only demo credentials.**
> They must NOT be used as real production credentials.
> 
> * The current authentication is temporary/demo-only.
> * There is currently no real backend authentication connected.
> * All external cloud database auth integrations have been completely removed.
> * These accounts are for local development and demonstration purposes only.

### Demo Accounts

#### Customer
```text
Email: customer@lokart.demo
Password: Customer@123
```

#### Delivery Partner
```text
Email: delivery@lokart.demo
Password: Delivery@123
```

#### Retailer
```text
Email: retailer@lokart.demo
Password: Retailer@123
```

---

## Project Experience

The LoKart frontend is designed around a cinematic, futuristic visual identity rather than a conventional e-commerce interface.

The landing experience introduces the LoKart ecosystem through:

**Darkness → Light Tunnel → LoKart → Local Commerce Network → Local Stores → Customers → Delivery → LoKart Ecosystem**

The visual design focuses on:

* Premium dark interface
* Futuristic typography
* Smooth transitions
* Interactive elements
* Scroll-based animations
* Local-commerce network visualization
* Responsive design
* Performance-conscious animations

The landing page is only one part of the overall LoKart platform.

---

## Technology Stack

### Frontend

* React
* TypeScript / JavaScript
* Tailwind CSS
* Modern CSS
* GSAP and/or Framer Motion
* Three.js / React Three Fiber where required

### Backend

The backend is intended to provide the APIs and services required by the LoKart platform, including:

* Authentication
* User management
* Shop management
* Product management
* Order management
* Delivery-related functionality

### Database

A database is used to store application data such as:

* Users
* Shops
* Products
* Orders
* Other platform information

---

## Project Structure

The project is organized into a clean, modular architecture:

```text
LoKart/
│
├── src/
│   ├── routes/              # TanStack Router page definitions (__root, index, customer, delivery, retailer)
│   ├── features/            # Domain features (auth, customer, delivery, retailer, landing)
│   ├── components/          # Shared base UI components (Radix UI / Shadcn)
│   ├── context/             # React Context Providers (AuthContext)
│   ├── services/            # Decoupled business service layer (AuthService)
│   ├── data/                # Static & Demo data definitions
│   ├── types/               # Shared TypeScript type interfaces
│   ├── hooks/               # Custom React hooks (useExploreLoKart, useMotionEnv, etc.)
│   ├── lib/                 # Core utilities & error reporting
│   └── styles/              # Global CSS styles
│
├── backend/                 # Backend API architecture placeholder
├── database/                # Database migrations & schemas placeholder
├── public/                  # Public static assets
├── .env.example             # Environment template
└── README.md
```

> The exact structure may change as development continues.

---

## Getting Started

### Prerequisites

Make sure the following are installed:

* Node.js
* npm
* Git
* Python
* pip

### Clone the Repository

```bash
git clone <repository-url>
cd LoKart
```

### Frontend

Navigate to the frontend directory:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The development server will provide a local URL in the terminal.

### Backend

Navigate to the backend directory:

```bash
cd backend
```

Create and activate a virtual environment:

```bash
python -m venv venv
```

Windows:

```bash
venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the backend server using the project's configured entry point.

---

## Environment Variables

If environment variables are required, create a `.env` file based on the project's configuration.

Example:

```env
DATABASE_URL=
API_URL=
JWT_SECRET=
```

Do not commit sensitive credentials or secret keys to the repository.

---

## Development

LoKart is actively developed as a full-stack local-commerce platform.

When contributing or extending the project:

1. Keep frontend and backend responsibilities separated.
2. Reuse existing components and services where possible.
3. Keep APIs organized and documented.
4. Maintain responsive behavior across devices.
5. Avoid unnecessary heavy animations.
6. Keep the user experience simple despite the visual complexity.
7. Do not commit secrets, credentials, or environment-specific configuration.

---

## Performance

The platform is designed to provide a smooth experience while maintaining the cinematic visual identity.

Performance considerations include:

* GPU-friendly animations
* Reduced animation complexity on mobile
* Lazy loading where appropriate
* Limited use of heavy WebGL effects
* Responsive layouts
* Reduced-motion support
* Efficient API usage

---

## Future Scope

Potential future improvements include:

* Real-time order tracking
* Advanced delivery partner integration
* Online payments
* Customer reviews and ratings
* Personalized recommendations
* Shop analytics
* Advanced admin dashboard
* Notifications
* Loyalty and rewards
* Improved location-based discovery
* Expansion to additional local commerce categories

---

## Project Vision

LoKart is built around a simple idea:

> **Local commerce should be connected, accessible, and digital without losing its local identity.**

The goal is to create an ecosystem where customers can easily discover nearby businesses while local shops gain a digital platform to serve and reach their communities.

**LoKart — Redefining Local Commerce.**

---

## License

This project is currently under development.

Add the appropriate license here when the project's licensing decision has been finalized.

---

## Credits

The project is independently developed and maintained as LoKart.
