# Code-A-Thon: AldenPark

A WWII Bletchley Park-inspired code-breaking hackathon website for Allegheny College.

## 🎯 About

Code-A-Thon: AldenPark is a 6-hour hackathon where student teams compete to decrypt classical ciphers using modern computational methods. The event combines historical cryptography with contemporary programming skills, inspired by the legendary codebreakers of Bletchley Park.

## 🎨 Design Features

- **1940s Military Aesthetic** - Vintage paper textures, military greens and browns
- **Neon Accents** - Modern cyan, pink, and green highlights for visual impact
- **Allegheny Colors** - Blue and gold integrated throughout
- **Typewriter/Monospace Fonts** - Period-appropriate typography
- **Morse Code Elements** - Decorative dividers and accents
- **Classified Document Styling** - Stamps, borders, and alert boxes

## 📁 Site Structure

```
├── Home Page (/) - Event overview and hero section
├── About (/about/) - Detailed event information and history
├── The Codes (/codes/)
│   ├── Caesar Cipher
│   ├── Vigenère Cipher
│   ├── Substitution Cipher
│   └── Mystery Cipher
├── Breaking Codes (/breaking/) - Cryptanalysis techniques and strategies
├── Rules & FAQ (/rules/) - Competition rules and frequently asked questions
├── Resources (/resources/) - Learning materials and tools
├── Contact (/contact/) - Organizer information
├── Announcements (/posts/) - News and updates
└── Search (/search/) - Site-wide search functionality
```

## 🚀 Getting Started

### Prerequisites

- Hugo Extended (v0.100.0 or later)
- Git

### Installation

1. Clone the repository:
```bash
git clone [repository-url]
cd bltechley
```

2. Initialize Git submodules (for theme):
```bash
git submodule update --init --recursive
```

3. Run the development server:
```bash
hugo server
```

4. Visit `http://localhost:1313` in your browser

### Building for Production

```bash
hugo --minify
```

The site will be built to the `public/` directory.

## 🎨 Customization

### Colors

The color scheme is defined in `assets/css/extended/custom.css`:

- **1940s Colors**: Military olive, brown, vintage paper, sepia
- **Allegheny Colors**: Blue (#003d7a), Gold (#fdb515)
- **Neon Accents**: Cyan, pink, green, orange

### Configuration

Main site settings are in `hugo.toml`:
- Site title and description
- Navigation menu
- Theme parameters
- Social links

### Content

All content is in Markdown format in the `content/` directory:
- `content/_index.md` - Home page
- `content/about.md` - About page
- `content/codes/*.md` - Cipher information pages
- `content/posts/*.md` - Announcements and news

## 📝 Adding Content

### New Announcement

```bash
hugo new posts/my-announcement.md
```

Edit the created file in `content/posts/my-announcement.md`

### New Page

```bash
hugo new page-name.md
```

## 🔧 Technical Details

### Theme

- **Base Theme**: PaperMod (highly customizable Hugo theme)
- **Custom CSS**: `assets/css/extended/custom.css`
- **Custom Layouts**: `layouts/partials/`

### Features

- ✅ Responsive design
- ✅ Search functionality
- ✅ Syntax highlighting for code examples
- ✅ Table of contents on long pages
- ✅ Fast page load times
- ✅ SEO optimized
- ✅ Print-friendly styles

## 📚 Content Highlights

### Cipher Pages

Each cipher page includes:
- Historical background
- How the cipher works
- Implementation examples in Python, JavaScript, and Java
- Breaking techniques and vulnerabilities
- Practice challenges with hints
- Advanced topics

### Breaking Guide

Comprehensive cryptanalysis resource covering:
- Frequency analysis
- Statistical measures (IC, chi-squared)
- Pattern recognition
- Attack methods for each cipher type
- Team strategies
- Code toolkit examples

## 🎯 Event Details

- **Name**: Code-A-Thon: AldenPark
- **Duration**: 6 hours
- **Date**: TBA (Saturday afternoon)
- **Location**: Allegheny College
- **Team Size**: 1-4 members
- **Challenges**: Caesar, Vigenère, Substitution, Mystery ciphers
- **Prizes**: T-shirts, awards, recognition

## 👥 Organizers

- **DataGators** - Allegheny College data science organization
- **ACM Club** - Association for Computing Machinery student chapter

## 📄 License

[Add your license information here]

## 🤝 Contributing

[Add contribution guidelines if this is a collaborative project]

## 📞 Contact

For questions about the event:
- Visit the [Contact Page](/contact/)
- Email: [Add contact email]

## 🙏 Acknowledgments

- Inspired by the codebreakers of Bletchley Park
- PaperMod Hugo theme
- Allegheny College Computer Science Department

---

## 🔐 Quick Commands

```bash
# Start development server
hugo server

# Start with drafts visible
hugo server -D

# Build for production
hugo --minify

# Check for errors
hugo --printPathWarnings

# Clean build cache
hugo --gc

# Update theme
git submodule update --remote --merge
```

---

**Built with Hugo 🚀 | Styled with Mystery 🔐 | Inspired by History 📜**

<div align="center">
  <strong>⚡ Break Codes. Build Software. Make History. ⚡</strong>
</div>
