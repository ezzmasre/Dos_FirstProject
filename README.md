# Bazar.com - Distributed Online Bookstore

A multi-tier, microservices-based online bookstore built with Node.js, implementing a RESTful architecture for distributed systems coursework.

## 📚 Project Overview

Bazar.com is a lightweight distributed bookstore system that demonstrates microservices architecture and REST API design. The system manages a catalog of four books across two topics and handles search, information retrieval, and purchase operations.

### Available Books

1. **How to get a good grade in DOS in 40 minutes a day** (Distributed Systems)
2. **RPCs for Noobs** (Distributed Systems)
3. **Xen and the Art of Surviving Undergraduate School** (Undergraduate School)
4. **Cooking for the Impatient Undergrad** (Undergraduate School)

## 🏗️ Architecture

The system follows a two-tier architecture with three independent microservices:

```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │
       v
┌─────────────────┐
│  Front-End      │
│  Server         │
└────┬───────┬────┘
     │       │
     v       v
┌─────────┐ ┌──────────┐
│ Catalog │ │  Order   │
│ Server  │ │  Server  │
└─────────┘ └────┬─────┘
                 │
                 v
            ┌─────────┐
            │ Catalog │
            │ Server  │
            └─────────┘
```

### Components

- **Front-End Server**: Handles client requests and routes them to appropriate backend services
- **Catalog Server**: Manages book inventory, pricing, and topic information
- **Order Server**: Processes purchase requests and maintains order history

## 🚀 Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- Docker (for containerized deployment)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/bazar-bookstore.git
cd bazar-bookstore
```

2. Install dependencies for each service:
```bash
# Front-end server
cd frontend
npm install

# Catalog server
cd ../catalog
npm install

# Order server
cd ../order
npm install
```

### Running Locally

1. Start the Catalog Server:
```bash
cd catalog
npm start
# Runs on http://localhost:3001
```

2. Start the Order Server:
```bash
cd order
npm start
# Runs on http://localhost:3002
```

3. Start the Front-End Server:
```bash
cd frontend
npm start
# Runs on http://localhost:3000
```

### Docker Deployment

Run all services using Docker containers:

```bash
# Build and start all containers
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all containers
docker-compose down
```

## 📡 API Documentation

### Front-End Server APIs

#### Search by Topic
```http
GET /search/:topic
```
**Example:**
```bash
curl http://localhost:3000/search/distributed%20systems
```
**Response:**
```json
{
  "items": {
    "How to get a good grade in DOS in 40 minutes a day": 1,
    "RPCs for Noobs": 2
  }
}
```

#### Get Book Information
```http
GET /info/:item_number
```
**Example:**
```bash
curl http://localhost:3000/info/2
```
**Response:**
```json
{
  "title": "RPCs for Noobs",
  "quantity": 5,
  "price": 50,
  "topic": "distributed systems"
}
```

#### Purchase Book
```http
POST /purchase/:item_number
```
**Example:**
```bash
curl -X POST http://localhost:3000/purchase/2
```
**Response:**
```json
{
  "success": true,
  "message": "Book purchased successfully",
  "order_id": "12345"
}
```

### Catalog Server APIs

#### Query by Topic
```http
GET /query/topic/:topic_name
```

#### Query by Item
```http
GET /query/item/:item_number
```

#### Update Catalog
```http
PUT /update/:item_number
```
**Body:**
```json
{
  "price": 55,
  "quantity": 10
}
```

### Order Server APIs

#### Process Purchase
```http
POST /purchase/:item_number
```

## 💾 Data Persistence

- Catalog data is stored in `catalog/data/catalog.csv`
- Order history is stored in `order/data/orders.csv`
- Both services automatically create and maintain these files

## 🧪 Testing

### Manual Testing

Use the provided test client:

```bash
cd client
node test-client.js
```

### Example Test Scenarios

1. **Search for books:**
```bash
curl http://localhost:3000/search/distributed%20systems
```

2. **Get book details:**
```bash
curl http://localhost:3000/info/1
```

3. **Purchase a book:**
```bash
curl -X POST http://localhost:3000/purchase/1
```

4. **Verify stock decrease:**
```bash
curl http://localhost:3000/info/1
```

## 📝 Configuration

Each service can be configured via environment variables:

### Front-End Server
```env
PORT=3000
CATALOG_SERVICE_URL=http://localhost:3001
ORDER_SERVICE_URL=http://localhost:3002
```

### Catalog Server
```env
PORT=3001
DATA_FILE=./data/catalog.csv
```

### Order Server
```env
PORT=3002
CATALOG_SERVICE_URL=http://localhost:3001
DATA_FILE=./data/orders.csv
```

## 🔧 Design Decisions

### Technology Stack
- **Node.js with Express**: Lightweight, efficient for RESTful microservices
- **CSV files**: Simple persistence without heavyweight databases
- **Docker**: Containerization for distributed deployment

### Key Features
- **Concurrent Request Handling**: Express handles multiple requests asynchronously
- **RESTful Design**: Clean HTTP-based communication between services
- **Separation of Concerns**: Each microservice has a single, well-defined responsibility
- **Stateless Services**: Easy to scale horizontally

### Tradeoffs
- CSV vs Database: Chose simplicity over query performance
- Synchronous vs Asynchronous: Order verification requires synchronous catalog check
- No transaction support: Edge cases with concurrent purchases may cause inconsistencies

## 🚧 Known Limitations

- Race condition possible when multiple clients purchase the last item simultaneously
- No authentication or authorization
- Limited error handling for network failures between services
- CSV parsing may be slow with large catalogs

## 🔮 Future Improvements

1. **Caching Layer**: Add Redis to cache frequently accessed catalog data
2. **Message Queue**: Implement RabbitMQ for asynchronous order processing
3. **Database Migration**: Move to PostgreSQL for better consistency guarantees
4. **Service Discovery**: Add Consul or etcd for dynamic service registration
5. **Load Balancing**: Add nginx for distributing traffic across multiple instances
6. **Monitoring**: Integrate Prometheus and Grafana for metrics
7. **Authentication**: Add JWT-based user authentication
8. **Inventory Reservation**: Implement two-phase commit for purchase transactions

## 📄 Project Structure

```
bazar-bookstore/
├── frontend/
│   ├── server.js
│   ├── package.json
│   └── Dockerfile
├── catalog/
│   ├── server.js
│   ├── data/
│   │   └── catalog.csv
│   ├── package.json
│   └── Dockerfile
├── order/
│   ├── server.js
│   ├── data/
│   │   └── orders.csv
│   ├── package.json
│   └── Dockerfile
├── client/
│   └── test-client.js
├── docs/
│   ├── design-document.pdf
│   └── sample-output.txt
├── docker-compose.yml
└── README.md
```

## 👥 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📜 License

This project is part of coursework for Distributed and Operating Systems - Spring 2019

## 🙏 Acknowledgments

- Course: Distributed and Operating Systems
- Assignment: Lab 1 - Multi-tier Online Book Store
- Framework: Express.js for Node.js microservices

---

**Note**: This is an educational project demonstrating distributed systems concepts including microservices architecture, REST APIs, and containerization.
