# Barangay Bulletin – Purok Diaz

A Laravel blog application for the Barangay Matina Crossing Hall where barangay officials publish announcements and residents can browse, search, and page through posts.

## About This Project

This is Lab Activity 10 for Full-Stack Web Development (4-334) at Ateneo de Davao University. The project demonstrates working with Laravel's Eloquent ORM, migrations, seeders, and pagination to build a functional bulletin board.

## Personalization

- **Site Title**: Barangay Bulletin – Purok Diaz
- **Kagawad**: Jan Dexter Diaz (kagawad.diaz@bulletin.test)
- **Pinned Post**: "Purok Diaz Clean-up Drive" (published January 23, 2026 — the Kagawad's birthday)
- **Posts Per Page**: 6 (since "Diaz" has 4 letters, the minimum of 6 is used)
- **Custom Category**: Livelihood (in addition to Announcement, Health, Peace & Order, and Ayuda)

## Setup & Installation

### Prerequisites
- PHP 8.5+
- Composer
- Node.js & npm
- SQLite (or use the database file that will be created)

### Installation Steps

```bash
# Clone the repository
git clone <repository-url>
cd LabActivity10-Diaz

# Install PHP dependencies
composer install

# Copy the environment file
cp .env.example .env

# Generate the application key
php artisan key:generate

# Create the SQLite database and run migrations with seeds
php artisan migrate:fresh --seed

# Install Node dependencies
npm install

# Build frontend assets (Tailwind CSS + Vite)
npm run build

# Start the Laravel development server
php artisan serve
```

Visit `http://localhost:8000` in your browser. The app redirects `/` to `/posts`.

To run the dev server with hot reload:
```bash
npm run dev
```

## Key Features

- **Browse Posts**: View all published posts sorted by pinned status and publication date
- **Search**: Filter posts by title using the search input
- **Filter by Category**: Filter posts by one of five categories
- **Pagination**: 6 posts per page with query string preservation (search and filter persist across pages)
- **View Counter**: Each post tracks and displays the number of views
- **Pinned Posts**: Special styling for pinned announcements (Purok Diaz Clean-up Drive)
- **Draft Posts**: Drafts are not visible to residents (404 error if accessed directly)

## Database Schema

### Authors Table
- `id` (primary key)
- `name` (string) — e.g., "Kagawad Jan Dexter Diaz"
- `position` (string) — e.g., "Kagawad", "Barangay Secretary"
- `email` (string, unique) — e.g., "kagawad.diaz@bulletin.test"
- `timestamps` (created_at, updated_at)

### Posts Table
- `id` (primary key)
- `author_id` (foreign key → authors.id, cascade on delete)
- `title` (string)
- `category` (string) — one of: Announcement, Health, Peace & Order, Ayuda, Livelihood
- `body` (text)
- `is_published` (boolean, default: false)
- `is_pinned` (boolean, default: false)
- `published_at` (timestamp, nullable)
- `views` (unsigned integer, default: 0) — added by separate migration
- `timestamps` (created_at, updated_at)

## Seeding

After running `php artisan migrate:fresh --seed`, the database contains:

- **5 Authors**: 1 Kagawad (created by hand) + 4 random officials
- **51 Posts**: 40 published, 10 drafts, plus the pinned "Purok Diaz Clean-up Drive" post

## Testing in Tinker

To verify Eloquent ORM functionality (ActiveRecord pattern):

```bash
php artisan tinker
>>> $post = App\Models\Post::find(1);
>>> $post->title = 'Updated Title';
>>> $post->save();
>>> App\Models\Post::find(1)->title  // Shows the updated title
```

Screenshot of this workflow is saved in `docs/tinker.png`.

## Technology Stack

- **Backend**: Laravel 12 with Eloquent ORM
- **Frontend**: Blade templates + Tailwind CSS v4
- **Database**: SQLite
- **Package Manager**: Composer (PHP), npm (Node)

## Notes

- No user authentication is required for this lab
- Relationships (hasMany/belongsTo) are deferred to Week 11; this app uses author IDs
- All output is escaped with `{{ }}` for security
- The app uses Laravel's built-in pagination views, styled with Tailwind
