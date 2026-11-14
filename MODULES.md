# Imagify - Additional Modules Documentation

This document describes all the new modules added to enhance the Imagify project.

mongodb+srv://greatstack:greatstack_03@cluster0.eu62vrg.mongodb.net/

## 📦 New Modules Overview

### 1. **Image Gallery Module**
- **Purpose**: Save and manage generated images
- **Features**:
  - Save generated images to database with metadata
  - View image history with pagination
  - Filter by favorites
  - Delete images
  - Toggle favorite status
- **API Endpoints**:
  - `GET /api/image/gallery` - Get user's image gallery
  - `GET /api/image/:id` - Get single image by ID
  - `PATCH /api/image/:id/favorite` - Toggle favorite status
  - `DELETE /api/image/:id` - Delete image
- **Frontend**: `/gallery` page

### 2. **Templates/Presets Module**
- **Purpose**: Pre-defined prompt templates for quick image generation
- **Features**:
  - Browse templates by category
  - Search templates
  - Use templates to generate images
  - Track template usage
  - Create custom templates
- **API Endpoints**:
  - `GET /api/templates` - Get all templates
  - `GET /api/templates/categories` - Get template categories
  - `GET /api/templates/:id` - Get template by ID
  - `POST /api/templates` - Create new template
  - `POST /api/templates/:id/use` - Use a template
- **Frontend**: `/templates` page
- **Seed Data**: Run `npm run seed:templates` to populate initial templates

### 3. **Admin Dashboard Module**
- **Purpose**: Administrative interface for managing the platform
- **Features**:
  - View system statistics (users, images, transactions, revenue)
  - Manage users (view, update credits, delete)
  - View all transactions
  - Popular prompts analytics
  - Daily statistics
- **API Endpoints**:
  - `GET /api/admin/users` - Get all users
  - `GET /api/admin/users/:id` - Get user details
  - `PATCH /api/admin/users/:id/credits` - Update user credits
  - `DELETE /api/admin/users/:id` - Delete user
  - `GET /api/admin/transactions` - Get all transactions
  - `POST /api/admin/templates` - Manage templates
- **Frontend**: `/admin` page

### 4. **Analytics Module**
- **Purpose**: Track and analyze platform usage
- **Features**:
  - Dashboard analytics (total users, images, transactions, revenue)
  - User-specific analytics
  - Popular prompts tracking
  - Daily statistics
  - Activity timeline
- **API Endpoints**:
  - `GET /api/analytics/dashboard` - Get dashboard analytics
  - `GET /api/analytics/user` - Get user analytics
  - `GET /api/analytics/popular-prompts` - Get popular prompts
- **Models**: Analytics events are automatically logged for:
  - Image generation
  - Payment transactions
  - Template usage
  - Batch generation
  - Variations generation

### 5. **Image Variations Module**
- **Purpose**: Generate variations of existing images
- **Features**:
  - Generate multiple variations from an existing image
  - Specify number of variations
  - Track credits used
- **API Endpoints**:
  - `POST /api/image/variations` - Generate variations
- **Request Body**:
  ```json
  {
    "imageId": "image_id_here",
    "count": 3
  }
  ```

### 6. **Batch Generation Module**
- **Purpose**: Generate multiple images at once
- **Features**:
  - Generate multiple images from an array of prompts
  - Handle partial failures gracefully
  - Track successful vs failed generations
- **API Endpoints**:
  - `POST /api/image/batch-generate` - Batch generate images
- **Request Body**:
  ```json
  {
    "prompts": ["prompt 1", "prompt 2", "prompt 3"]
  }
  ```

### 7. **Favorites Module**
- **Purpose**: Save and manage favorite images
- **Features**:
  - Mark images as favorites
  - Filter gallery by favorites
  - Quick access to favorite images
- **Integrated with**: Image Gallery module

### 8. **Notification Module**
- **Purpose**: User notification system
- **Features**:
  - Real-time notifications
  - Mark as read/unread
  - Notification types (info, success, warning, error)
  - Notification links
  - Unread count badge
- **API Endpoints**:
  - `GET /api/notifications` - Get user notifications
  - `PATCH /api/notifications/:id/read` - Mark as read
  - `PATCH /api/notifications/read-all` - Mark all as read
  - `DELETE /api/notifications/:id` - Delete notification
- **Frontend**: Notification bell component in Navbar
- **Auto-notifications**: Created for:
  - Image generation success
  - Payment success
  - Credit updates

### 9. **Rate Limiting Module**
- **Purpose**: API protection and abuse prevention
- **Features**:
  - General API rate limiting (100 requests per 15 minutes)
  - Image generation rate limiting (50 requests per hour)
  - Authentication rate limiting (5 attempts per 15 minutes)
- **Middleware**: `express-rate-limit`
- **Applied to**: All routes with appropriate limits

### 10. **Image History Module**
- **Purpose**: Track and view generation history
- **Features**:
  - Complete generation history
  - Metadata tracking (generation time, credits used)
  - Timestamp tracking
- **Integrated with**: Image Gallery module

## 🗄️ Database Models

### Image Model
```javascript
{
  userId: ObjectId,
  prompt: String,
  imageUrl: String (base64),
  isFavorite: Boolean,
  tags: [String],
  metadata: {
    width: Number,
    height: Number,
    format: String,
    creditsUsed: Number
  },
  generationTime: Number
}
```

### Template Model
```javascript
{
  name: String,
  category: String,
  prompt: String,
  description: String,
  thumbnail: String,
  isPublic: Boolean,
  usageCount: Number,
  createdBy: ObjectId
}
```

### Analytics Model
```javascript
{
  userId: ObjectId,
  eventType: String,
  metadata: Object,
  timestamp: Date
}
```

### Notification Model
```javascript
{
  userId: ObjectId,
  title: String,
  message: String,
  type: String (info|success|warning|error),
  isRead: Boolean,
  link: String
}
```

## 🚀 Getting Started

### 1. Install Dependencies
```bash
cd server
npm install express-rate-limit
```

### 2. Seed Templates (Optional)
```bash
npm run seed:templates
```

### 3. Start the Server
```bash
npm run server
```

### 4. Access New Features
- Gallery: `http://localhost:5173/gallery`
- Templates: `http://localhost:5173/templates`
- Admin: `http://localhost:5173/admin`

## 📝 API Usage Examples

### Generate Image with Gallery Save
```javascript
POST /api/image/generate-image
Headers: { token: "user_token" }
Body: { prompt: "A beautiful sunset" }
```

### Get User Gallery
```javascript
GET /api/image/gallery?page=1&limit=20&favorite=false
Headers: { token: "user_token" }
```

### Use Template
```javascript
POST /api/templates/:templateId/use
Headers: { token: "user_token" }
```

### Generate Variations
```javascript
POST /api/image/variations
Headers: { token: "user_token" }
Body: { imageId: "image_id", count: 3 }
```

### Batch Generate
```javascript
POST /api/image/batch-generate
Headers: { token: "user_token" }
Body: { prompts: ["prompt1", "prompt2", "prompt3"] }
```

## 🔒 Security Features

- Rate limiting on all endpoints
- Authentication middleware for protected routes
- User-specific data isolation
- Input validation

## 📊 Analytics Events

The system automatically tracks:
- `image_generated` - When an image is generated
- `variations_generated` - When variations are created
- `batch_generation` - When batch generation occurs
- `template_used` - When a template is used
- `payment_made` - When payment is completed

## 🎨 Frontend Components

- **Gallery.jsx** - Image gallery with filters and pagination
- **Templates.jsx** - Template browser and selector
- **Admin.jsx** - Admin dashboard with tabs
- **NotificationBell.jsx** - Notification dropdown component

## 🔄 Future Enhancements

Potential additions:
- Image editing capabilities
- Social sharing features
- User profiles and portfolios
- Advanced search and filtering
- Image export in multiple formats
- Collaborative features
- API documentation (Swagger)
- Webhook support
- Email notifications
- Image upscaling
- Style transfer

## 📞 Support

For issues or questions, please refer to the main project documentation or create an issue in the repository.