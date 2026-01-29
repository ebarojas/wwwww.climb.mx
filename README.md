                                                                                      
```                                                                             
            ,--,                      ____                           ____              
          ,--.'|     ,--,           ,'  , `.  ,---,                ,'  , `.            
          |  | :   ,--.'|        ,-+-,.' _ |,---.'|             ,-+-,.' _ |            
          :  : '   |  |,      ,-+-. ;   , |||   | :          ,-+-. ;   , ||,--,  ,--,  
   ,---.  |  ' |   `--'_     ,--.'|'   |  ||:   : :         ,--.'|'   |  |||'. \/ .`|  
  /     \ '  | |   ,' ,'|   |   |  ,', |  |,:     |,-.     |   |  ,', |  |,'  \/  / ;  
 /    / ' |  | :   '  | |   |   | /  | |--' |   : '  |     |   | /  | |--'  \  \.' /   
.    ' /  '  : |__ |  | :   |   : |  | ,    |   |  / :     |   : |  | ,      \  ;  ;   
'   ; :__ |  | '.'|'  : |__ |   : |  |/     '   : |: |     |   : |  |/      / \  \  \  
'   | '.'|;  :    ;|  | '.'||   | |`-'      |   | '/ :___  |   | |`-'     ./__;   ;  \ 
|   :    :|  ,   / ;  :    ;|   ;/          |   :    /  .\ |   ;/         |   :/\  \ ; 
 \   \  /  ---`-'  |  ,   / '---'           /    \  /\  ; |'---'          `---'  `--`  
  `----'            ---`-'                  `-'----'  `--"                             
```                                    

# climb.mx blog a simple personal website

A minimalist Rails blog application with a minimal web design aesthetic, designed to get you writing 
and collecting emails from your readers so you can push your updates to them via email or similar.

## 🎨 Design Philosophy

This blog embraces the raw, authentic feel of early web design:
- Basic HTML structure with minimal CSS
- Retro color schemes and typography
- Simple, functional layouts
- No JavaScript frameworks (vanilla only)
- Responsive design with a 90s twist

## 🚀 Features

- **Blog Posts**: Create, edit, and display blog posts
- **Email Collection**: Visitor email signup for newsletters
- **PostgreSQL Database**: Robust data storage
- **90s Aesthetic**: Retro web design with modern functionality
- **Docker Support**: Easy deployment and development
- **Jekyll style blogposts**: Blog posts are md files, totally cross compatible with Jekyll
- **SEO Optimized**: Meta tags, Open Graph, structured data, and dynamic sitemap
- **LLM Attribution**: JSON-LD structured data and ai.txt for proper AI crawler attribution

> 📖 **SEO Documentation**: See [SEO_Features.md](SEO_Features.md) for comprehensive documentation on SEO features, LLM attribution, helper methods, and best practices.

## 🛠 Tech Stack

- **Ruby on Rails**
- **PostgreSQL+**
- **Ruby 3.2**
- **Docker & Docker Compose**
- **Vanilla HTML/CSS/JavaScript**

## 🏗 Quick Start

### Using Docker (Recommended)

1. **Build and Start**
   ```bash
   docker compose up --build
   ```

Use without --build unless gemfile or dockerfile have changed. To stop:

   ```bash
   docker compose down
   ```

2. **Setup Database**
   ```bash
   docker compose exec web rails db:create db:migrate db:seed
   ```

3. **Access the Application**
   - Blog: http://localhost:3000
   - Admin: http://localhost:3000/admin

## ✍️ Creating New Posts

This blog uses Jekyll-style markdown files for posts. Use the included script to quickly create new posts:

### First Time Setup
Make the script executable:
```bash
chmod +x bin/new-post
```

### Usage

**Basic Post Creation:**
```bash
./bin/new-post "Your Post Title Here"
```

**Post with Images:**
```bash
./bin/new-post "Your Post Title Here" -i folder_name
```

**Examples:**
```bash
# Basic post
./bin/new-post "My Amazing Rails Tutorial"

# Post with images from a folder
./bin/new-post "My Photo Gallery" -i vacation_photos
```

This creates: `app/posts/2025-01-27-my-amazing-rails-tutorial.md`

### What the Script Does
- Generates filename with current date and title (spaces → dashes)
- Creates proper Jekyll front matter with:
  - `title`: Exact title you provided
  - `date`: Current date and time
  - `permalink`: Title converted to snake_case
  - `categories`: "general" (placeholder)
  - `description`: "placeholder" (placeholder) - **Important for SEO!**
- Places the file in `app/posts/` directory

**SEO Note:** The `description` field is used for SEO meta descriptions. Replace "placeholder" with a compelling 150-160 character description that summarizes your post. If omitted, the system will automatically truncate your content, but a well-written description performs better in search results. See [SEO_Features.md](SEO_Features.md) for more details.

### Image Integration Feature (`-i` flag)

When using the `-i` flag, the script automatically includes all images from the specified folder:

**Requirements:**
- Folder must exist in `/public/imgs/`
- Only image files are processed (jpg, jpeg, png, gif, webp)
- Images are sorted alphabetically

**What happens:**
1. Script validates the folder exists in `/public/imgs/`
2. Scans for image files and ignores non-image files
3. Generates markdown image tags with captions:
   ```markdown
   ![filename](/imgs/folder/filename.jpg)
   *filename*
   ```
4. Adds an "Images" section to the post content

**Error handling:**
- If folder doesn't exist: "Folder not found, aborting"
- Script fails gracefully without creating the post

### Manual Fallback
If the script isn't working, you can create posts manually:
```bash
# Create the file
touch app/posts/$(date +%Y-%m-%d)-your-title-here.md

# Add front matter manually
cat > app/posts/$(date +%Y-%m-%d)-your-title-here.md << 'EOF'
---
layout: post
title:  "Your Title Here"
date:   $(date +"%Y-%m-%d %H:%M:%S +0000")
categories: general
description: placeholder
permalink: /your_title_here/
---

# Your Title Here

Your content goes here...
EOF
```

## 📁 Project Structure

```
ror.climb.mx/
├── app/
│   ├── controllers/
│   │   ├── application_controller.rb
│   │   ├── pages_controller.rb
│   │   ├── posts_controller.rb
│   │   ├── sitemap_controller.rb
│   │   └── subscribers_controller.rb
│   ├── models/
│   │   ├── application_record.rb
│   │   ├── post.rb
│   │   └── subscriber.rb
│   ├── views/
│   │   ├── layouts/
│   │   │   ├── application.html.erb
│   │   │   ├── mailer.html.erb
│   │   │   └── mailer.text.erb
│   │   ├── pages/
│   │   │   ├── about.html.erb
│   │   │   ├── contact.html.erb
│   │   │   └── home.html.erb
│   │   ├── posts/
│   │   │   ├── index.html.erb
│   │   │   └── show.html.erb
│   │   ├── sitemap/
│   │   │   └── index.xml.erb        # Dynamic sitemap (not in public/)
│   │   ├── subscribers/
│   │   │   └── new.html.erb
│   │   └── pwa/
│   │       ├── manifest.json.erb
│   │       └── service-worker.js
│   ├── assets/
│   │   ├── images/
│   │   └── stylesheets/
│   │       └── application.css
│   ├── posts/                    # Markdown blog posts (Jekyll-style)
│   │   ├── 2025-01-31-Books-I-read-in-2024.md
│   │   ├── 2024-04-08-Climbing-Rope-Mat.md
│   │   └── ... (other .md files)
│   ├── drafts/                   # Draft posts
│   ├── helpers/
│   │   └── seo_helper.rb         # SEO helper methods
│   ├── jobs/
│   └── mailers/
├── config/
│   ├── application.rb
│   ├── routes.rb
│   ├── database.yml
│   ├── environments/
│   ├── initializers/
│   └── locales/
├── db/
│   ├── migrate/
│   ├── schema.rb
│   └── seeds.rb
├── public/
│   ├── imgs/                     # Blog post images
│   ├── robots.txt                # Search engine crawler instructions
│   ├── ai.txt                    # AI crawler attribution information
│   ├── 404.html
│   └── favicon files
│   # Note: sitemap.xml is dynamically generated at /sitemap.xml (not a static file)
├── resources/                    # Project resources and documentation
│   ├── Cursor_instructions.md    # Technical documentation for Ulysses project
│   ├── Ulysses_Dialog_Compendium.md  # Extracted dialogue reference
│   └── ulysses.txt              # Source text (Project Gutenberg)
├── .github/
│   └── workflows/
│       └── ci.yml
├── bin/                          # Rails binstubs
├── lib/
├── log/
├── storage/
├── tmp/
├── vendor/
├── Dockerfile
├── docker-compose.yml
├── docker-compose.prod.yml
├── Gemfile
├── README.md
└── DEVELOPMENT.md
```

## 🗄 Database Schema

### Subscribers Table
- `id` (Primary Key)
- `email` (String, unique)
- `name` (Boolean, default: false)
- `about` (Text)
- `created_at` (DateTime)
- `updated_at` (DateTime)

## 🔧 Configuration

### Environment Variables
Create a `.env` file in the root directory:

```env
DATABASE_URL=postgresql://postgres:password@db:5432/blog_app
RAILS_ENV=development
SECRET_KEY_BASE=your-secret-key-here
```

### Database Configuration
The application uses PostgreSQL with the following default settings:
- Host: `db` (Docker) or `localhost`
- Port: `5432`
- Database: `blog_app`
- Username: `postgres`
- Password: `password`


## 📧 Newsletter Features (coming at some point)

- Email collection with confirmation
- Admin interface for managing subscribers
- Newsletter sending capabilities (future feature)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Troubleshooting

### Common Issues

**Database Connection Error**
```bash
docker compose exec web rails db:create
```

**Port Already in Use**
```bash
# Change port in docker-compose.yml
ports:
  - "3001:3000"
```

**Permission Denied**
```bash
sudo chown -R $USER:$USER .
```

## 📞 Support

For issues and questions:
- Create an issue in the repository
- Check the troubleshooting section
- Review Rails documentation

---

Built with ❤️ from the geocities and net art days.
