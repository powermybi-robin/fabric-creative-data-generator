# 🎭 AI-Powered Themed Star Schema Generator for Microsoft Fabric

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Microsoft Fabric](https://img.shields.io/badge/Microsoft-Fabric-blue.svg)](https://www.microsoft.com/microsoft-fabric)
[![Azure OpenAI](https://img.shields.io/badge/Azure-OpenAI-green.svg)](https://azure.microsoft.com/products/ai-services/openai-service)

Generate synthetic analytical datasets with **Azure OpenAI** and **Apache Spark** in **Microsoft Fabric**. Perfect for demos, training, testing, and prototyping!

## 🌟 Overview

This notebook revolutionizes demo database creation by combining AI creativity with data engineering best practices. Instead of boring "Product_042" and "Store_17", generate engaging datasets like:

- 🦴 **Caveman Fine Dining** - "Primordial Bone Marrow Broth" at "Mammoth Mountain Restaurant"
- 🚀 **Space Colony Retail** - "Nebula Nutrient Packs" from "Mars Station Alpha"
- 🧙 **Fantasy Marketplace** - "Dragon's Breath Soup" at "Wizard's Tower Boutique"

Your imagination is the only limit!

## ✨ Features

- **🤖 AI-Powered Content** - Azure OpenAI (GPT-4o) generates creative themed names
- **⚡ Spark Processing** - Efficient large-scale data generation with PySpark
- **🎨 Multiple Business Types** - Retail, Restaurant, Healthcare (easily extensible)
- **🎭 Any Theme You Want** - Cyberpunk, Medieval, Underwater, Time Travel, etc.
- **🔐 Secure Credentials** - Direct key config with security reminders
- **📦 Fallback Mode** - Works without API access using pre-loaded themes
- **📊 Production-Ready Schema** - Complete star schema with fact and dimension tables
- **🎲 Reproducible** - Seeded random generation for consistent results
- **💾 Delta Lake Format** - Optimized for Power BI Direct Lake mode

## 🚀 Quick Start

### Prerequisites

- **Microsoft Fabric** workspace with a Lakehouse
- **Azure OpenAI** deployment (optional - fallback mode available)
- **Fabric Capacity** F64 or higher recommended

### Installation

1. Download `Fabric_Notebook_with_Generative_Data_Licensed.ipynb`
2. Upload to your Fabric workspace
3. Open in Fabric and attach to your Lakehouse
4. Configure your Azure OpenAI credentials (or use Preloaded mode)
5. Run all cells!

### Basic Configuration

```python
# Choose your business type
business_type = "Restaurant"  # or "Retail", "Healthcare"

# Pick any creative theme
theme = "Cyberpunk Street Food"

# Use AI or pre-loaded content
generation_mode = "AI"  # or "Preloaded"

# Choose dataset size
record_scale = "medium"  # "small" (10K), "medium" (25K), "large" (500K)
```

## 📊 Generated Schema

The notebook creates a complete star schema optimized for analytics:

### Dimension Tables

| Table | Description | Rows |
|-------|-------------|------|
| `demo_dim_date` | Complete date dimension (2022-2027) | ~2,190 |
| `demo_dim_product` | Products/services with categories, brands | 100+ |
| `demo_dim_location` | Geographic locations across US regions | 100 |

### Fact Tables

| Business Type | Fact Table | Description |
|---------------|------------|-------------|
| Retail | `demo_fact_sales` | Sales transactions |
| Restaurant | `demo_fact_orders` | Food orders |
| Healthcare | `demo_fact_visits` | Patient visits |

All tables are written in **Delta Lake** format for optimal performance and Power BI Direct Lake compatibility.

## 🎨 Creative Theme Examples

### Restaurant Themes
- "Caveman Fine Dining"
- "Cyberpunk Noodle Bar"
- "Underwater Seafood Palace"
- "Medieval Tavern"
- "Robot Chef Bistro"

### Retail Themes
- "Space Colony Supply Store"
- "Wizard's Marketplace"
- "Steampunk Workshop"
- "Time Traveler's Boutique"
- "Dinosaur Theme Park Shop"

### Healthcare Themes
- "Alien Embassy Clinic"
- "Future Med Center 2150"
- "Dragon Healing Sanctuary"
- "Cyborg Repair Station"

## 💡 Use Cases

### 📊 Demonstrations & Presentations
- Create engaging demo data for conference talks
- Build memorable presentations that audiences love
- Show off Fabric capabilities with creative datasets

### 🎓 Training & Education
- Generate practice datasets for Power BI training
- Teach SQL with fun, relatable data
- Create Spark/PySpark learning exercises

### 🧪 Testing & Development
- Produce realistic test data for applications
- Stress test Power BI reports with large datasets
- Validate ETL pipelines with known data patterns

### 🎨 Prototyping
- Quickly create themed datasets for POCs
- Build customer demos with their industry context
- Test data models before production implementation

## 🔒 Security Best Practices

**⚠️ IMPORTANT**: This notebook includes direct API key configuration for demo convenience.

### After Every Demonstration:
1. **Regenerate your Azure OpenAI API key immediately**
2. Never commit real keys to version control
3. Use `.gitignore` to exclude credential files

### Production Recommendations:
- Use **Azure Key Vault** for credential storage
- Implement **Managed Identity** authentication
- Store keys in **environment variables**
- Rotate keys regularly

## 📈 Sample Analytics

Once data is generated, try these queries:

```sql
-- Revenue by Category
SELECT 
    p.category,
    SUM(f.revenue) as total_revenue,
    COUNT(*) as transactions
FROM demo_fact_orders f
JOIN demo_dim_product p ON f.product_key = p.product_key
GROUP BY p.category
ORDER BY total_revenue DESC;

-- Monthly Trends
SELECT 
    d.year,
    d.month_name,
    SUM(f.revenue) as revenue,
    SUM(f.profit) as profit
FROM demo_fact_orders f
JOIN demo_dim_date d ON f.date_key = d.date_key
GROUP BY d.year, d.month, d.month_name
ORDER BY d.year, d.month;

-- Top Locations
SELECT 
    l.location_name,
    l.city,
    SUM(f.revenue) as total_revenue
FROM demo_fact_orders f
JOIN demo_dim_location l ON f.location_key = l.location_key
GROUP BY l.location_name, l.city
ORDER BY total_revenue DESC
LIMIT 10;
```

## 🔧 Configuration Options

| Parameter | Options | Description |
|-----------|---------|-------------|
| `business_type` | Retail, Restaurant, Healthcare | Industry vertical to simulate |
| `theme` | Any text | Creative theme for naming |
| `generation_mode` | AI, Preloaded | Use Azure OpenAI or built-in content |
| `record_scale` | small, medium, large | 10K, 25K, or 500K transactions |
| `random_seed` | Any integer | For reproducible datasets |
| `direct_key` | Your API key | Azure OpenAI authentication |

## 🤝 Contributing

Contributions are welcome! Here are some ways to help:

### Add New Business Types
- Healthcare specializations (Dental, Vision, etc.)
- Financial services
- Education/Training
- Entertainment/Events

### Create Pre-loaded Themes
- Add more built-in themes to the fallback mode
- Cultural themes from around the world
- Historical periods
- Literary/pop culture references

### Improve Performance
- Optimize Spark operations
- Add partitioning strategies
- Implement Z-ordering suggestions

### Enhance Documentation
- Add video tutorials
- Create Power BI template files
- Write blog posts about use cases

## 🐛 Troubleshooting

### "Unterminated string" Error
**Problem**: JSON response truncated  
**Solution**: Increase `max_tokens` to 8000 or 16000 in Step 4

### Authentication Failed (401)
**Problem**: Azure OpenAI credentials invalid  
**Solutions**:
- Verify endpoint URL is correct
- Check API key hasn't expired
- Confirm deployment name matches
- Regenerate key if needed

### Table Not Found When Refreshing
**Problem**: Hardcoded table names don't match  
**Solution**: Use the dynamic refresh cell that auto-detects tables

### Out of Memory
**Problem**: Dataset too large for capacity  
**Solutions**:
- Reduce `record_scale` to "small" or "medium"
- Upgrade Fabric capacity tier
- Adjust `spark.sql.shuffle.partitions`

### Slow Performance
**Problem**: Large dataset generation takes too long  
**Solutions**:
- Start with "small" scale for testing
- Use larger Fabric capacity
- Run during off-peak hours

## 📚 Resources

- [Microsoft Fabric Documentation](https://learn.microsoft.com/fabric/)
- [Power BI Direct Lake Guide](https://learn.microsoft.com/power-bi/enterprise/directlake-overview)
- [Azure OpenAI Service](https://learn.microsoft.com/azure/ai-services/openai/)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Delta Lake Documentation](https://docs.delta.io/)

## 📧 Author

**Robin Abramson**  
*Power BI & Microsoft Fabric Specialist*

- 20+ years in Business Intelligence
- Power BI & Microsoft Fabric expert
- Semantic model & advanced visualization specialist

## 📜 License

This project is licensed under the **MIT License** - see below for details.

```
MIT License

Copyright (c) 2025 Robin Abramson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 🌟 Acknowledgments

- **Microsoft Fabric Team** - For an amazing data platform
- **Azure OpenAI** - For making creative AI accessible
- **Apache Spark Community** - For powerful data processing
- **Power BI Community** - For inspiration and feedback

## 🎉 Show Your Support

If this notebook helped you:
- ⭐ Star this repository
- 📣 Share with colleagues
- 🐛 Report issues
- 💡 Suggest new features
- 🎨 Share your creative themes!

---

**Made with ❤️ for the Data Community**

*Remember: Always regenerate your Azure OpenAI keys after demonstrations!* 🔐
