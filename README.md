# Inventory Management System

A full-stack learning management platform built with Next.js, Express.js, and AWS services. This system allows instructors to create and manage courses while students can enroll, track progress, and make payments.

## 🏗️ Architecture

This project consists of two main components:

- **Client**: Next.js 15 frontend application with modern React features
- **Server**: Express.js backend API with AWS DynamoDB integration

## 🚀 Features

### For Students
- Browse and search courses by category
- Secure payment processing with Stripe
- Track course progress and completion
- Video streaming and interactive content
- User authentication with Clerk

### For Instructors
- Create and manage courses
- Upload video content to AWS S3
- Track student enrollments and progress
- Organize content into sections and chapters
- Support for multiple content types (Video, Text, Quiz)

### Technical Features
- **Authentication**: Clerk integration for secure user management
- **Payments**: Stripe integration for course purchases
- **File Storage**: AWS S3 for video and image storage
- **Database**: DynamoDB for scalable data storage
- **CDN**: CloudFront for fast content delivery
- **Deployment**: AWS Lambda for serverless backend

## 📁 Project Structure

```
├── client/                 # Next.js frontend application
│   ├── src/
│   │   ├── app/           # Next.js app router pages
│   │   └── components/    # Reusable React components
│   ├── public/            # Static assets
│   └── package.json       # Frontend dependencies
│
├── server/                 # Express.js backend API
│   ├── src/
│   │   ├── controllers/   # API route handlers
│   │   ├── models/        # DynamoDB data models
│   │   ├── routes/        # Express route definitions
│   │   ├── seed/          # Database seeding scripts
│   │   └── utils/         # Utility functions
│   ├── Dockerfile         # Docker configuration for AWS Lambda
│   └── package.json       # Backend dependencies
│
└── README.md              # This file
```

## 🛠️ Technology Stack

### Frontend
- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS with custom design system
- **UI Components**: Radix UI primitives
- **Authentication**: Clerk
- **Payments**: Stripe React components
- **State Management**: Redux Toolkit
- **Animations**: Framer Motion

### Backend
- **Runtime**: Node.js with Express.js
- **Language**: TypeScript
- **Database**: AWS DynamoDB with Dynamoose ODM
- **Authentication**: Clerk Express middleware
- **File Storage**: AWS S3
- **Payments**: Stripe API
- **Deployment**: AWS Lambda (serverless)

### AWS Services
- **DynamoDB**: Primary database for courses, users, and transactions
- **S3**: File storage for videos and images
- **CloudFront**: CDN for fast content delivery
- **Lambda**: Serverless function hosting

## 🚦 Getting Started

### Prerequisites

- Node.js 18+ and npm
- AWS account with configured credentials
- Clerk account for authentication
- Stripe account for payments

### Environment Variables

Create the following environment files:

#### Client (.env.local)
```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
```

#### Server (.env)
```env
NODE_ENV=development
PORT=3001
CLERK_SECRET_KEY=your_clerk_secret_key
STRIPE_SECRET_KEY=your_stripe_secret_key
AWS_REGION=us-east-2
S3_BUCKET_NAME=your_s3_bucket_name
CLOUDFRONT_DOMAIN=your_cloudfront_domain
```

### Installation & Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd inventory-management
   ```

2. **Install dependencies**
   ```bash
   # Install client dependencies
   cd client
   npm install

   # Install server dependencies
   cd ../server
   npm install
   ```

3. **Set up DynamoDB Local (Development)**
   ```bash
   # Install DynamoDB Local
   npm install -g dynamodb-local
   
   # Start DynamoDB Local
   dynamodb-local
   ```

4. **Seed the database**
   ```bash
   cd server
   npm run seed
   ```

5. **Start the development servers**
   ```bash
   # Terminal 1: Start the backend server
   cd server
   npm run dev

   # Terminal 2: Start the frontend application
   cd client
   npm run dev
   ```

6. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:3001

## 📊 Database Schema

### Tables

#### Courses
- **courseId** (Primary Key): Unique course identifier
- **teacherId**: Instructor's user ID
- **title**: Course title
- **description**: Course description
- **category**: Course category
- **price**: Course price in cents
- **level**: Difficulty level (Beginner, Intermediate, Advanced)
- **status**: Publication status (Draft, Published)
- **sections**: Array of course sections with chapters

#### Transactions
- **userId** (Hash Key): Student's user ID
- **transactionId** (Range Key): Stripe payment intent ID
- **courseId**: Associated course ID
- **amount**: Payment amount in cents
- **paymentProvider**: Payment processor (stripe)
- **dateTime**: Transaction timestamp

#### UserCourseProgress
- **userId** (Hash Key): Student's user ID
- **courseId** (Range Key): Course ID
- **overallProgress**: Completion percentage (0-100)
- **sections**: Progress tracking for each section/chapter
- **enrollmentDate**: When the user enrolled
- **lastAccessedTimestamp**: Last activity timestamp

## 🔧 API Endpoints

### Courses
- `GET /courses` - List all courses (with optional category filter)
- `GET /courses/:courseId` - Get specific course details
- `POST /courses` - Create new course (authenticated)
- `PUT /courses/:courseId` - Update course (authenticated)
- `DELETE /courses/:courseId` - Delete course (authenticated)
- `POST /courses/:courseId/sections/:sectionId/chapters/:chapterId/get-upload-url` - Get S3 upload URL

### Transactions
- `GET /transactions` - List transactions (with optional user filter)
- `POST /transactions` - Create new transaction
- `POST /transactions/stripe/payment-intent` - Create Stripe payment intent

### User Progress
- `GET /users/course-progress/:userId/enrolled-courses` - Get user's enrolled courses
- `GET /users/course-progress/:userId/courses/:courseId` - Get specific course progress
- `PUT /users/course-progress/:userId/courses/:courseId` - Update course progress

### User Management
- `PUT /users/clerk/:userId` - Update user metadata

## 🔐 Authentication & Authorization

The application uses Clerk for authentication with the following features:

- **Email/Password Authentication**: Standard sign-up and sign-in
- **Protected Routes**: Server-side authentication middleware
- **User Roles**: Support for different user types (student, instructor)
- **Session Management**: Automatic token handling

### Authorization Rules
- Only course owners can update/delete their courses
- Users can only access their own progress data
- Payment transactions are user-specific

## 💳 Payment Integration

Stripe integration provides:

- **Secure Payment Processing**: PCI-compliant payment handling
- **Payment Intents**: Modern payment flow with 3D Secure support
- **Transaction Tracking**: Complete payment history
- **Automatic Enrollment**: Course access granted after successful payment

## 📱 Responsive Design

The frontend is built with a mobile-first approach featuring:

- **Breakpoint System**: Optimized for mobile, tablet, and desktop
- **Custom Design System**: Consistent spacing, colors, and typography
- **Accessibility**: WCAG compliant components
- **Dark Mode Support**: Theme switching capability

## 🚀 Deployment

### Frontend (Vercel/Netlify)
```bash
cd client
npm run build
```

### Backend (AWS Lambda)
```bash
cd server
npm run build
# Deploy using AWS CLI or serverless framework
```

### Environment Setup
1. Configure AWS credentials
2. Set up Clerk production environment
3. Configure Stripe webhook endpoints
4. Update environment variables for production

## 🧪 Development

### Running Tests
```bash
# Frontend tests
cd client
npm run test

# Backend tests
cd server
npm run test
```

### Database Seeding
```bash
cd server
npm run seed
```

### Building for Production
```bash
# Build client
cd client
npm run build

# Build server
cd server
npm run build
```

## 📝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style
- Use TypeScript for all new code
- Follow the existing file organization patterns
- Maintain consistent naming conventions
- Add proper error handling and validation

## 🐛 Troubleshooting

### Common Issues

**DynamoDB Connection Issues**
- Ensure DynamoDB Local is running on port 8000
- Check AWS credentials configuration
- Verify region settings match your setup

**Authentication Problems**
- Verify Clerk environment variables
- Check webhook configurations
- Ensure proper middleware setup

**Payment Issues**
- Confirm Stripe keys are correctly set
- Check webhook endpoint configuration
- Verify payment intent creation

### Debug Mode
Set `NODE_ENV=development` to enable:
- Detailed error logging
- Local DynamoDB connection
- Development-specific configurations

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Support

For support and questions:
- Create an issue in the repository
- Check the troubleshooting section
- Review the API documentation

## 🔄 Version History

- **v1.0.0**: Initial release with core functionality
- Course management and enrollment system
- Stripe payment integration
- User progress tracking
- AWS infrastructure setup