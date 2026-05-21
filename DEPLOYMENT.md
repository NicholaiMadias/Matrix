# Subdomain Deployment Guide

This document provides instructions for deploying the Matrix of Conscience project across three subdomains.

## Subdomain Structure

The project is organized into three subdomains:

- **library.matrix.amazinggracehl.org** - Knowledge Repository
- **theater.matrix.amazinggracehl.org** - Performance & Presentation
- **galleries.matrix.amazinggracehl.org** - Visual Collections

## Directory Structure

```
Matrix/
├── index.html              # Main landing page
├── library/
│   └── index.html         # Library subdomain
├── theater/
│   └── index.html         # Theater subdomain
└── galleries/
    └── index.html         # Galleries subdomain
```

## Deployment Options

### Option 1: Web Server Configuration (Apache)

Create virtual hosts for each subdomain:

```apache
<VirtualHost *:80>
    ServerName library.matrix.amazinggracehl.org
    DocumentRoot /var/www/matrix/library
    <Directory /var/www/matrix/library>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName theater.matrix.amazinggracehl.org
    DocumentRoot /var/www/matrix/theater
    <Directory /var/www/matrix/theater>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName galleries.matrix.amazinggracehl.org
    DocumentRoot /var/www/matrix/galleries
    <Directory /var/www/matrix/galleries>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName matrix.amazinggracehl.org
    DocumentRoot /var/www/matrix
    <Directory /var/www/matrix>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

### Option 2: Web Server Configuration (Nginx)

Create server blocks for each subdomain:

```nginx
server {
    listen 80;
    server_name library.matrix.amazinggracehl.org;
    root /var/www/matrix/library;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name theater.matrix.amazinggracehl.org;
    root /var/www/matrix/theater;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name galleries.matrix.amazinggracehl.org;
    root /var/www/matrix/galleries;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name matrix.amazinggracehl.org;
    root /var/www/matrix;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Option 3: GitHub Pages with Custom Domains

If deploying to GitHub Pages:

1. Enable GitHub Pages in repository settings
2. Set up custom domains in Settings > Pages
3. Add CNAME records for each subdomain:
   - `library.matrix` → `yourusername.github.io`
   - `theater.matrix` → `yourusername.github.io`
   - `galleries.matrix` → `yourusername.github.io`

### Option 4: Cloudflare Pages

1. Connect your GitHub repository to Cloudflare Pages
2. Set the build output directory to `/`
3. Configure custom domains for each subdomain
4. Add the following routing rules:
   - `library.matrix.amazinggracehl.org/*` → `/library/*`
   - `theater.matrix.amazinggracehl.org/*` → `/theater/*`
   - `galleries.matrix.amazinggracehl.org/*` → `/galleries/*`

## DNS Configuration

Add the following DNS records to your domain registrar:

```
Type    Name                              Value
A       matrix.amazinggracehl.org         [Your Server IP]
CNAME   library.matrix.amazinggracehl.org matrix.amazinggracehl.org
CNAME   theater.matrix.amazinggracehl.org matrix.amazinggracehl.org
CNAME   galleries.matrix.amazinggracehl.org matrix.amazinggracehl.org
```

Or for direct IP mapping:

```
Type    Name                              Value
A       matrix.amazinggracehl.org         [Your Server IP]
A       library.matrix.amazinggracehl.org [Your Server IP]
A       theater.matrix.amazinggracehl.org [Your Server IP]
A       galleries.matrix.amazinggracehl.org [Your Server IP]
```

## SSL/TLS Configuration

For production deployments, enable HTTPS using Let's Encrypt:

### Using Certbot (Apache/Nginx)

```bash
# For Apache
sudo certbot --apache -d matrix.amazinggracehl.org -d library.matrix.amazinggracehl.org -d theater.matrix.amazinggracehl.org -d galleries.matrix.amazinggracehl.org

# For Nginx
sudo certbot --nginx -d matrix.amazinggracehl.org -d library.matrix.amazinggracehl.org -d theater.matrix.amazinggracehl.org -d galleries.matrix.amazinggracehl.org
```

## Testing

After deployment, test each subdomain:

- https://matrix.amazinggracehl.org (Main landing page)
- https://library.matrix.amazinggracehl.org (Library subdomain)
- https://theater.matrix.amazinggracehl.org (Theater subdomain)
- https://galleries.matrix.amazinggracehl.org (Galleries subdomain)

## Navigation

Each subdomain includes:
- A link back to the main landing page (← Home)
- Cross-links to other subdomains at the bottom of the page
- Consistent branding and styling

## Maintenance

- All pages use CDN-hosted dependencies (Tailwind CSS)
- No build process required
- Pages are static HTML with embedded JavaScript
- Updates can be made by editing the HTML files directly
