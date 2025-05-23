# Integration App Demo

This is a demonstration application showcasing integration capabilities using [Integration.app](https://integration.app). The app is built with Next.js and demonstrates how to implement user authentication, integration token generation, and data synchronization with external systems.

## Prerequisites

- Node.js 18+ installed
- Integration.app workspace credentials (Workspace Key and Secret)
- MongoDB database (for storing records and schemas)

## Setup

1. Clone the repository:

```bash
git clone <repository-url>
cd <repository-name>
```

2. Install dependencies:

```bash
npm install
# or
yarn install
```

3. Set up environment variables:

```bash
# Copy the sample environment file
cp .env-sample .env
```

4. Edit `.env` and add your Integration.app credentials:

```env
INTEGRATION_APP_WORKSPACE_KEY=your_workspace_key_here
INTEGRATION_APP_WORKSPACE_SECRET=your_workspace_secret_here
MONGODB_URI=your_mongodb_connection_string
```

You can find these credentials in your Integration.app workspace settings.

## Configuration

### Webhook URLs

The application sends webhook notifications for record operations (create, update, delete). You can configure the webhook URLs in `src/lib/webhook-utils.ts`:

```typescript
// Define webhook URLs for default record types
const WEBHOOK_URLS = {
  tasks: 'https://api.integration.app/webhooks/app-events/4bd9f7cc-f295-4eac-bd78-d051bc127f59',
  contacts: 'https://api.integration.app/webhooks/app-events/7cb4f624-01f5-4662-b886-68b9461b8b0a',
  companies: 'https://api.integration.app/webhooks/app-events/cd9f4430-8f4e-45a4-badd-9d4666078540',
  // Default URL for custom objects
  custom: 'https://api.integration.app/webhooks/app-events/19c8f829-0723-4030-9164-95398285f5da'
};
```

Replace these URLs with your own webhook endpoints from Integration.app.

### Record Types

Default record types are defined in `src/lib/constants.ts`. You can modify this file to add or remove record types:

```typescript
export const RECORD_ACTIONS = [
  { key: 'get-contacts', label: 'Contacts', type: 'default' },
  { key: 'get-companies', label: 'Companies', type: 'default' },
  { key: 'get-tasks', label: 'Tasks', type: 'default' },
  // Add more record types as needed
];
```

## Running the Application

1. Start the development server:

```bash
npm run dev
# or
yarn dev
```

2. Open [http://localhost:3000](http://localhost:3000) in your browser.

## Key Features

### Authentication

The application uses a simple authentication mechanism with a customer ID. In a production environment, you would replace this with your own authentication system.

### Integrations

The Integrations page allows users to connect to external applications using Integration.app. Once connected, you can import data from these applications.

### Forms

The Forms page allows you to create and manage custom forms with dynamic fields. You can:
- Create new form types
- Add custom fields to forms
- Delete fields from forms

### Records

The Records page displays data imported from external applications. You can:
- View records by type
- Edit records
- Delete records
- Import new records

### Submit Form

The Submit Form page allows users to submit data through custom forms. The submitted data is sent to the appropriate webhook URL based on the form type.

## Project Structure

- `/src/app` - Next.js app router pages and API routes
  - `/api` - Backend API routes for records, schemas, and integration tokens
  - `/records` - Record management pages and components
  - `/forms` - Form management pages and components
  - `/integrations` - Integration connection pages
  - `/submit-form` - Form submission page
- `/src/components` - Reusable React components
- `/src/hooks` - Custom React hooks
- `/src/lib` - Utility functions and helpers
- `/src/types` - TypeScript type definitions

## API Endpoints

- `/api/records` - CRUD operations for records
- `/api/schema/[formId]/[userId]` - Form schema management
- `/api/forms` - Form definition management
- `/api/integration-token` - Integration token generation
- `/api/self` - User information

## Webhook Payloads

When records are created, updated, or deleted, the application sends webhook notifications with the following structure:

```json
{
  "type": "created|updated|deleted",
  "data": {
    "id": "record-id",
    "recordType": "record-type",
    "fields": { /* record fields */ }
  },
  "customerId": "customer-id"
}
```

For custom record types, an additional `instanceKey` property is included in the payload.

## Troubleshooting

- **MongoDB Connection Issues**: Ensure your MongoDB connection string is correct and the database is accessible.
- **Integration Token Errors**: Verify your Integration.app workspace credentials in the `.env` file.
- **Webhook Errors**: Check the webhook URLs in `webhook-utils.ts` and ensure they are correctly configured in Integration.app.

## License

MIT
