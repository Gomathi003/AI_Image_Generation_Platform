# Admin Access Setup Guide

## How to Login to Admin Page

### Step 1: Make a User Admin

To access the admin dashboard, you first need to make a user account an admin. Follow these steps:

1. **Make sure you have a registered user account** - Register/login normally through the app

2. **Run the make-admin script** from the server directory:
   ```bash
   cd server
   npm run make-admin <your-email>
   ```
   
   Example:
   ```bash
   npm run make-admin admin@example.com
   ```

3. **You should see a success message**:
   ```
   ✅ Successfully made John Doe (admin@example.com) an admin!
   ```

### Step 2: Login and Access Admin Page

1. **Login** to the app with your admin account (the email you just made admin)

2. **Navigate to Admin Dashboard**:
   - Click the "Admin" link in the navbar (only visible to admin users)
   - Or go directly to: `http://localhost:5173/admin`

3. **You'll see the Admin Dashboard** with:
   - Dashboard tab: System statistics (users, images, transactions, revenue)
   - Users tab: Manage all users, update credits, delete users
   - Transactions tab: View all payment transactions

## Admin Features

### Dashboard
- View total users, images, transactions, and revenue
- See recent activity and popular prompts
- Daily statistics

### User Management

- Update user credits (add or set)
- Delete users
- View user details and statistics

### Transaction Management
- View all payment transactions
- Filter by status (paid/pending)
- See transaction details

## Security

- Admin routes are protected by `adminAuth` middleware
- Only users with `isAdmin: true` or `role: 'admin'` can access admin endpoints
- Regular users will see "Access Denied" if they try to access admin pages

## Troubleshooting

### "Admin access required" error
- Make sure you ran the `make-admin` script with the correct email
- Verify the user exists in the database
- Try logging out and logging back in after making yourself admin

### Admin link not showing in navbar
- Make sure you're logged in with an admin account
- Check that `isAdmin` is set to `true` in the database
- Refresh the page after making yourself admin

### Can't access admin routes
- Verify your token is valid (try logging out and back in)
- Check that the admin middleware is working correctly
- Ensure the user document has `isAdmin: true` or `role: 'admin'`

## Database Check

You can verify admin status directly in MongoDB:
```javascript
db.users.findOne({ email: "your-email@example.com" })
// Should show: { isAdmin: true, role: "admin" }
```