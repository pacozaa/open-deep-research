# User Approval System

This application implements a user approval system where new user registrations require administrator approval before they can access the system.

## How It Works

1. **User Registration**: When a user registers through `/register`, their account is created with `isApproved` set to `false` by default
2. **Login Restriction**: Users with `isApproved = false` cannot log in and will see an error message stating their account is pending approval
3. **Administrator Approval**: An administrator must manually approve users by updating the database

## Approving Users

### Using Database Client

Connect to your PostgreSQL database and run:

```sql
-- Approve a specific user by email
UPDATE "User" 
SET "isApproved" = true 
WHERE email = 'user@example.com';

-- List all pending users
SELECT id, email, "isApproved" 
FROM "User" 
WHERE "isApproved" = false;

-- Approve all pending users (use with caution)
UPDATE "User" 
SET "isApproved" = true 
WHERE "isApproved" = false;
```

### Using Drizzle Studio

You can also use Drizzle Studio to manage user approvals:

```bash
pnpm db:studio
```

This will open a web interface where you can:
1. Navigate to the "User" table
2. Find the user you want to approve
3. Set the `isApproved` field to `true`
4. Save the changes

## User Experience

### Registration
- Users fill out the registration form
- Upon successful registration, they see: "Account created successfully! Your account is pending approval."
- They are not automatically logged in

### Login Attempts
- Unapproved users attempting to login will see: "Your account is pending approval. Please wait for an administrator to approve your account."
- Approved users can login normally

## Migration

The `isApproved` field was added via migration `0006_sour_zaladane.sql`. 

**Important**: Existing users in the database before this migration will have `isApproved = false` by default. You may want to run:

```sql
-- Approve all existing users
UPDATE "User" 
SET "isApproved" = true 
WHERE "isApproved" = false;
```

## Future Enhancements

Potential improvements to consider:

1. **Admin Dashboard**: Create a web interface for managing user approvals
2. **Email Notifications**: Notify users when their account is approved
3. **Auto-approval**: Add configuration to auto-approve users from specific domains
4. **Approval Workflow**: Add approval reasons, rejection capabilities, and audit logs
