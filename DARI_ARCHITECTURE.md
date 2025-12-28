# 🏠 Dari - Moroccan Real Estate Platform

## 📋 Overview

**Dari** is a modern real estate application designed specifically for the Moroccan market, enabling users to rent or buy properties with ease. The platform supports three distinct user roles: Clients, Property Owners, and Real Estate Agents.

---

## 🎯 App Goal

Dari aims to simplify the real estate experience in Morocco by providing:
- Easy property discovery for renters and buyers
- Simple property listing management for owners
- Professional tools for real estate agents
- Direct communication between parties (WhatsApp & Phone)

---

## 👥 User Roles

### 1. **Client** (المستأجر / المشتري)
**Capabilities:**
- Browse and search properties
- Filter by location, price, type, and features
- Save favorite properties
- View detailed property information
- Contact property owners directly

### 2. **Owner** (المالك)
**Capabilities:**
- All client features
- Publish new property listings
- Manage existing listings
- Respond to client inquiries
- Update property information

### 3. **Agent** (الوسيط العقاري)
**Capabilities:**
- All owner features
- Manage multiple properties on behalf of different owners
- Track commissions
- Professional property management tools

---

## ⭐ Core Features (Implemented)

### ✅ Authentication & User Management
- Role-based access control
- User profiles with contact information
- Secure login/logout functionality

### ✅ Property Browsing
- Grid view of all available properties
- Property cards with key information
- High-quality property images
- Transaction type badges (Rent/Buy)

### ✅ Advanced Search & Filtering
- Filter by transaction type (rent/buy)
- Filter by property type (apartment, villa, house, studio, office, land)
- Filter by city (all major Moroccan cities)
- Price range filtering
- Area (m²) range filtering
- Minimum bedrooms filtering
- Collapsible advanced filters

### ✅ Property Details
- Full-screen property view
- Complete property information
- Owner contact details
- Direct communication buttons (WhatsApp & Phone)
- Add/remove from favorites

### ✅ Favorites System
- Save properties to favorites
- Dedicated favorites page
- Quick access to saved properties
- Visual indicators for favorited items

### ✅ Add Property (Owners/Agents)
- Comprehensive property form
- All property types supported
- Image upload capability
- Validation for required fields
- Success notifications

### ✅ User Profile
- Personal information display
- Role-specific descriptions
- Contact information
- Account management

### ✅ Responsive Design
- Mobile-first approach
- Tablet optimization
- Desktop full features
- Touch-friendly interface

---

## 🗄️ Database Schema

### **Users Table**
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  phone VARCHAR(20) NOT NULL,
  role VARCHAR(20) NOT NULL CHECK (role IN ('client', 'owner', 'agent')),
  avatar_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### **Properties Table**
```sql
CREATE TABLE properties (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title VARCHAR(500) NOT NULL,
  description TEXT NOT NULL,
  price DECIMAL(12, 2) NOT NULL,
  transaction_type VARCHAR(10) NOT NULL CHECK (transaction_type IN ('rent', 'buy')),
  property_type VARCHAR(20) NOT NULL CHECK (property_type IN ('apartment', 'villa', 'house', 'studio', 'office', 'land')),
  city VARCHAR(100) NOT NULL,
  area DECIMAL(10, 2) NOT NULL, -- in m²
  bedrooms INTEGER,
  bathrooms INTEGER,
  images TEXT[], -- Array of image URLs
  owner_id UUID REFERENCES users(id) ON DELETE CASCADE,
  featured BOOLEAN DEFAULT FALSE,
  status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'pending', 'sold', 'rented')),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### **Favorites Table**
```sql
CREATE TABLE favorites (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  property_id UUID REFERENCES properties(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(user_id, property_id)
);
```

### **Contacts/Inquiries Table** (Future)
```sql
CREATE TABLE inquiries (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  property_id UUID REFERENCES properties(id) ON DELETE CASCADE,
  client_id UUID REFERENCES users(id) ON DELETE CASCADE,
  message TEXT,
  status VARCHAR(20) DEFAULT 'new' CHECK (status IN ('new', 'contacted', 'closed')),
  created_at TIMESTAMP DEFAULT NOW()
);
```

### **Agent Commissions Table** (Future)
```sql
CREATE TABLE commissions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  agent_id UUID REFERENCES users(id) ON DELETE CASCADE,
  property_id UUID REFERENCES properties(id) ON DELETE CASCADE,
  amount DECIMAL(10, 2) NOT NULL,
  percentage DECIMAL(5, 2),
  status VARCHAR(20) DEFAULT 'pending' CHECK (status IN ('pending', 'paid')),
  created_at TIMESTAMP DEFAULT NOW(),
  paid_at TIMESTAMP
);
```

---

## 🔄 User Flows

### Guest User Flow
```
1. Visit Homepage
2. Browse Properties (Grid View)
3. Apply Filters (City, Type, Price)
4. Click Property → View Details
5. Contact Owner (WhatsApp/Call)
   OR
6. Prompted to Login → Save to Favorites
```

### Client Flow
```
1. Login as Client
2. Browse & Filter Properties
3. Save Favorites ❤️
4. View Property Details
5. Contact Owner
6. Manage Favorites List
7. View Profile
```

### Owner Flow
```
1. Login as Owner
2. Click "Add Property" ➕
3. Fill Property Form
   - Title, Description
   - Price, Type, Location
   - Images, Features
4. Submit → Property Published
5. View Own Listings
6. Receive Client Contacts
7. Manage Properties
```

### Agent Flow
```
1. Login as Agent
2. Add Multiple Properties
3. Manage All Listings
4. Track Commissions
5. Professional Dashboard
6. Client Communication
```

---

## 🖼️ Screens & Components

### Main Screens
1. **Home Page** (`/`)
   - Navbar with logo and navigation
   - Search filters sidebar
   - Property grid (responsive)
   - Featured properties

2. **Favorites Page** (`/favorites`)
   - Saved properties grid
   - Quick remove from favorites
   - Empty state message

3. **Property Details Modal**
   - Large property image
   - Full description
   - All property specs
   - Owner information
   - Contact buttons (WhatsApp, Phone)

4. **Add Property Form** (`/add-property`)
   - Multi-field form
   - Image upload
   - Validation
   - Success feedback

5. **User Profile** (`/profile`)
   - User information
   - Contact details
   - Role badge
   - Account settings

6. **Login Modal**
   - Role selection
   - Demo user selection
   - Role explanations

### Key Components
- `Navbar` - Navigation and authentication
- `PropertyCard` - Property preview card
- `PropertyDetails` - Full property modal
- `SearchFilters` - Advanced search sidebar
- `AddPropertyForm` - Property creation form
- `UserProfile` - User account page
- `LoginModal` - Authentication modal

---

## 🎨 Design System

### Color Palette
- **Primary (Emerald)**: `#059669` - Main actions, links, highlights
- **Secondary (Blue)**: `#2563EB` - Rental properties, info
- **Success (Green)**: `#10B981` - Success messages, sold
- **Warning (Amber)**: `#F59E0B` - Pending states
- **Error (Red)**: `#EF4444` - Errors, delete actions
- **Gray Scale**: Modern neutral grays for text and backgrounds

### Typography
- Modern, clean sans-serif font
- Hierarchical heading sizes
- Readable body text
- Arabic/French bilingual support ready

### UI Components
- Rounded corners (modern aesthetic)
- Soft shadows for depth
- Hover states for interactivity
- Smooth transitions
- Mobile-first responsive grid

---

## 🚀 Future Improvements & Monetization

### Phase 2 - Enhanced Features
1. **Multi-language Support**
   - Arabic (العربية)
   - French (Français)
   - English

2. **Image Gallery**
   - Multiple images per property
   - Carousel/slideshow
   - Zoom functionality

3. **Map Integration**
   - Google Maps integration
   - Property location markers
   - Nearby amenities

4. **Advanced Filters**
   - Parking availability
   - Furnished/Unfurnished
   - Pet-friendly
   - Amenities (pool, garden, elevator, etc.)

5. **Property Comparison**
   - Compare up to 3 properties
   - Side-by-side specifications
   - Price comparison

### Phase 3 - Professional Features
1. **Virtual Tours**
   - 360° property views
   - Video walkthroughs
   - 3D floor plans

2. **Property Management**
   - Edit existing listings
   - Mark as sold/rented
   - View statistics (views, contacts)
   - Bulk operations

3. **Messaging System**
   - In-app chat
   - Message history
   - Read receipts
   - Push notifications

4. **Verification System**
   - Verified property owners
   - Agent certification
   - Property authenticity badges

5. **Reviews & Ratings**
   - Property reviews
   - Owner ratings
   - Agent feedback

### Phase 4 - Monetization Strategy

#### **Freemium Model**
- **Free Tier:**
  - Browse all properties
  - Contact owners
  - Save favorites (limited)
  
- **Premium for Owners:** (299 DH/month)
  - Unlimited listings
  - Featured property placement
  - Priority in search results
  - Advanced analytics
  
- **Pro for Agents:** (799 DH/month)
  - Unlimited properties
  - CRM tools
  - Commission tracking
  - Professional badge
  - Lead management

#### **Commission-Based**
- 1-2% commission on successful transactions
- Payment gateway integration
- Escrow services

#### **Advertising**
- Sponsored listings
- Banner ads for related services (movers, banks, insurance)
- Featured developer promotions

#### **Additional Services**
- Professional photography packages
- Legal documentation assistance
- Property valuation services
- Moving services marketplace

### Phase 5 - Advanced Platform Features
1. **AI-Powered Recommendations**
   - Personalized property suggestions
   - Price prediction
   - Market insights

2. **Mobile Apps**
   - Native iOS app
   - Native Android app
   - Push notifications

3. **Admin Dashboard**
   - User management
   - Content moderation
   - Analytics & reporting
   - Revenue tracking

4. **Integration Ecosystem**
   - Mortgage calculators
   - Bank partnerships
   - Insurance providers
   - Legal services

---

## 🛠️ Technical Stack (Current)

### Frontend
- **React 18** - UI framework
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **Lucide React** - Icons
- **Radix UI** - Accessible components
- **Sonner** - Toast notifications

### Future Stack (Production)

#### Backend
- **Supabase** (Recommended)
  - PostgreSQL database
  - Authentication & authorization
  - Row-level security
  - Real-time subscriptions
  - File storage for images
  - RESTful API

#### Alternative Backend Options
- **Firebase** - Google's platform
- **AWS Amplify** - Amazon's solution
- **Custom Node.js/Express** - Full control

#### Infrastructure
- **Vercel/Netlify** - Frontend hosting
- **Cloudinary** - Image optimization & CDN
- **SendGrid** - Email notifications
- **Twilio** - SMS notifications

---

## 📊 Success Metrics (KPIs)

### User Engagement
- Daily Active Users (DAU)
- Monthly Active Users (MAU)
- Average session duration
- Properties viewed per session
- Favorite conversion rate

### Business Metrics
- Total listings published
- Successful transactions
- Revenue per user
- Customer acquisition cost (CAC)
- Lifetime value (LTV)

### Quality Metrics
- User satisfaction score
- Property quality rating
- Response time to inquiries
- Listing completion rate

---

## 🔐 Security & Privacy

### Current Implementation
- Client-side data validation
- Role-based access control
- Secure contact information handling

### Future Security Features
- End-to-end encryption for messages
- Two-factor authentication (2FA)
- Phone number verification
- Email verification
- GDPR compliance
- Data encryption at rest
- Regular security audits
- Rate limiting
- DDoS protection

---

## 🌍 Moroccan Market Specifics

### Localization
- Prices in Moroccan Dirham (DH)
- Major Moroccan cities pre-populated
- Arabic & French language support
- Local payment methods (cash, bank transfer)

### Cultural Considerations
- WhatsApp integration (primary communication)
- Phone call option (preferred by many)
- Family-oriented property descriptions
- Ramadan-specific features (seasonal)

### Legal Compliance
- Moroccan real estate law compliance
- Property registration verification
- Agency licensing requirements
- Consumer protection laws

---

## 📱 Responsive Breakpoints

- **Mobile**: 0-640px (sm)
- **Tablet**: 641px-1024px (md, lg)
- **Desktop**: 1025px+ (xl, 2xl)

---

## 🎯 Competitive Advantages

1. **Morocco-First Design** - Built specifically for Moroccan users
2. **Simple & Fast** - No unnecessary complexity
3. **Direct Communication** - WhatsApp integration
4. **Modern UX** - Clean, intuitive interface
5. **Mobile-Optimized** - Most Moroccans browse on mobile
6. **Free to Start** - Low barrier to entry
7. **Bilingual Support** - Arabic & French

---

## 📈 Roadmap Timeline

### Q1 2025 - Foundation ✅
- Core property listing features
- Search & filtering
- User authentication
- Favorites system

### Q2 2025 - Enhancement
- Backend integration (Supabase)
- Multi-language support
- Map integration
- Advanced filters

### Q3 2025 - Monetization
- Premium subscriptions
- Featured listings
- Payment integration
- Analytics dashboard

### Q4 2025 - Scale
- Mobile apps
- AI recommendations
- Marketing automation
- Partnership integrations

---

## 🤝 Support & Contact

For questions, support, or collaboration:
- **App Name**: Dari (داري - "My Home" in Arabic)
- **Target Market**: Morocco
- **Languages**: Arabic, French, English (future)

---

## 📝 Notes

This is currently a **frontend prototype** with mock data. To build a production-ready application, you'll need:

1. **Backend Database** (Supabase recommended)
2. **Authentication System** (Email/phone verification)
3. **Image Storage & CDN** (Cloudinary or Supabase Storage)
4. **Payment Gateway** (for premium features)
5. **Email/SMS Service** (for notifications)

The current implementation demonstrates all core UI/UX features and can be easily connected to a backend service.

---

**Built with ❤️ for the Moroccan real estate market**
