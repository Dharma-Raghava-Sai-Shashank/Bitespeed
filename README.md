# Bitespeed Customer Identity Service

This service helps track and identify customers across multiple purchases by linking their contact information (email and phone number).

## Features

- Identifies customers based on email and/or phone number
- Links multiple contact records for the same customer
- Maintains a primary contact record with secondary linked records
- Handles merging of contact information across different orders

## Prerequisites

- Node.js (v14 or higher)
- PostgreSQL database
- npm or yarn package manager

## Setup

1. Clone the repository
2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory with the following variables:

   ```
   DATABASE_URL="postgresql://username:password@localhost:5432/bitespeed?schema=public"
   PORT=3000
   ```

4. Initialize the database:

   ```bash
   npx prisma migrate dev
   ```

5. Start the development server:
   ```bash
   npm run dev
   ```

## API Endpoints

### POST /identify

Identifies or creates a customer contact based on provided email and/or phone number.

**Request Body:**

```json
{
  "email": "string",
  "phoneNumber": "string"
}
```

**Response:**

```json
{
  "contact": {
    "primaryContatctId": number,
    "emails": string[],
    "phoneNumbers": string[],
    "secondaryContactIds": number[]
  }
}
```

## Development

- `npm run dev` - Start development server with hot reload
- `npm run build` - Build for production
- `npm start` - Start production server
- `npm test` - Run tests

## Database Schema

The service uses a PostgreSQL database with the following schema:

```prisma
model Contact {
  id              Int       @id @default(autoincrement())
  phoneNumber     String?
  email           String?
  linkedId        Int?
  linkPrecedence  String    @default("primary")
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt
  deletedAt       DateTime?
}
```
