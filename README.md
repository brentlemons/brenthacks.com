# brenthacks.com Blog - Requirements Document

## Project Overview
Building a blog website for brenthacks.com using Hugo static site generator. The site will serve as the official blog for the Brent Hacks YouTube channel, featuring coding adventures, tutorials, and video content. The site will be managed with Git, hosted on AWS S3, distributed via CloudFront, and use the existing brenthacks.com domain managed in Route 53.

## Key Requirements
- **Static Site Generator**: Hugo (extended version for SCSS/SASS support)
- **Version Control**: Git with GitHub repository
- **Hosting**: AWS S3 (private bucket)
- **CDN**: AWS CloudFront (with Origin Access Control)
- **Domain**: brenthacks.com via Route 53
- **Security**: S3 bucket not publicly accessible, HTTPS only

## Implementation Phases

### Phase 1: Local Development Setup
1. **Install Hugo** (if not already installed)
   - Use extended version for SCSS/SASS support
   
2. **Initialize Hugo Site**
   - Create new Hugo site structure
   - Choose a clean, fast blog theme (e.g., PaperMod, Ananke, or custom)
   - Configure `config.toml` with site metadata

### Phase 2: Git & GitHub Setup
1. **Initialize Git Repository**
   - Create `.gitignore` for Hugo (public/, resources/, .hugo_build.lock)
   - Initial commit with base structure
   
2. **Create GitHub Repository**
   - Public repository for open source transparency
   - Add README with project documentation
   - Push local repository to GitHub using SSH

### Phase 3: Content Development & Iteration
1. **Content Structure**
   - Set up content directories (`content/posts/`, `content/about/`)
   - Create archetypes for consistent post formatting
   - Design homepage layout
   
2. **Initial Content Creation**
   - Write first blog posts
   - Create about page
   - Add any additional pages (projects, contact, etc.)
   
3. **Theme Customization**
   - Customize colors, fonts, layouts
   - Add logo/branding
   - Configure navigation menus
   - Optimize for mobile responsiveness
   
4. **Local Testing & Refinement**
   - Iterative development with `hugo server -D`
   - Test all functionality
   - Refine design and content
   - Ensure fast page load times

### Phase 4: AWS Infrastructure
1. **S3 Bucket Configuration**
   - Create bucket (e.g., `brenthacks-com-content`)
   - **Block all public access** (crucial for security)
   - Enable static website hosting
   - Configure bucket policy to allow CloudFront access only (using OAC - Origin Access Control)
   
2. **CloudFront Distribution**
   - Create distribution with S3 origin
   - Configure Origin Access Control (OAC) for secure S3 access
   - Set default root object to `index.html`
   - Configure error pages (404.html)
   - Enable compression
   - Set appropriate cache behaviors
   
3. **SSL/TLS Certificate**
   - Request ACM certificate for brenthacks.com (must be in us-east-1 for CloudFront)
   - Include www.brenthacks.com as alternate name
   - Validate via DNS (Route 53)

### Phase 5: Domain Configuration
1. **Route 53 Setup**
   - Create A record (ALIAS) pointing to CloudFront distribution
   - Optional: Create www subdomain redirect
   - Configure proper TTL values

### Phase 6: Deployment Pipeline
1. **GitHub Actions Workflow**
   - Build Hugo site on push to main
   - Upload to S3 with `aws s3 sync`
   - Invalidate CloudFront cache
   - Use OIDC for secure AWS authentication (no long-lived keys)
   
2. **AWS IAM Configuration**
   - Create IAM role for GitHub Actions
   - Minimal permissions: S3 put/delete for specific bucket, CloudFront invalidation
   - Configure OIDC trust relationship

### Phase 7: Testing & Monitoring
1. **Production Testing**
   - Verify CloudFront caching
   - Test SSL certificate
   - Check DNS propagation
   
2. **Monitoring Setup**
   - CloudWatch alarms for 4xx/5xx errors
   - S3 access logging
   - CloudFront access logs

## Security Best Practices
- S3 bucket remains private (no public access)
- CloudFront OAC ensures only CloudFront can access S3
- All traffic forced through HTTPS
- Security headers in CloudFront (HSTS, X-Frame-Options)
- Regular dependency updates for Hugo themes

## Cost Optimization
- CloudFront caching to minimize S3 requests
- Appropriate cache headers for static assets
- Consider S3 lifecycle policies for logs
- Use CloudFront price class for required regions only

## Progress Tracking

### Completed
- [x] Phase 1: Local Development Setup
  - ✓ Hugo extended version installed (v0.147.9)
  - ✓ Hugo site initialized
  - ✓ Logbook-hugo theme installed from existing project
  - ✓ Basic configuration completed (baseURL, title, metadata)
  - ✓ Development server tested and working
- [x] Phase 2: Git & GitHub Setup
  - ✓ Git repository initialized
  - ✓ `.gitignore` file created with Hugo-specific exclusions
  - ✓ Initial commit completed with descriptive message
  - ✓ GitHub repository created: https://github.com/brentlemons/brenthacks.com
  - ✓ Repository pushed to GitHub using SSH
- [ ] Phase 3: Content Development & Iteration
- [ ] Phase 4: AWS Infrastructure
- [ ] Phase 5: Domain Configuration
- [ ] Phase 6: Deployment Pipeline
- [ ] Phase 7: Testing & Monitoring

## Change Log
_Document any changes to requirements as the project progresses_

- **2025-08-01**: Initial requirements document created
- **2025-07-31 22:27:02 CDT**: Phase 1 completed - Hugo site initialized with logbook-hugo theme
- **2025-07-31 22:36:56 CDT**: Phase 2 completed - Git repository and GitHub setup completed
  - Updated project description to reflect Brent Hacks YouTube channel focus
  - Repository made public for open source transparency
  - SSH authentication configured for GitHub access