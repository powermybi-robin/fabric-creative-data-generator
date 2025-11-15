# 🚀 Setup & Customization Guide

This guide walks you through setting up the AI-Powered Themed Star Schema Generator in Microsoft Fabric, configuring Azure OpenAI, and customizing the notebook for your needs.

---

## 📋 Table of Contents

1. [Setting Up Microsoft Fabric](#setting-up-microsoft-fabric)
2. [Importing the Notebook](#importing-the-notebook)
3. [Setting Up Azure OpenAI](#setting-up-azure-openai)
4. [First Run](#first-run)
5. [Customization Guide](#customization-guide)
6. [Advanced Modifications](#advanced-modifications)
7. [Troubleshooting](#troubleshooting)

---

## 🏗️ Setting Up Microsoft Fabric

### Step 1: Get Access to Fabric

**If you don't have Fabric yet:**

1. **Free Trial (60 days)**
   - Visit [Microsoft Fabric Trial](https://app.fabric.microsoft.com/)
   - Click "Start free trial"
   - Sign in with your Microsoft account
   - You get a free F64 capacity for 60 days!

2. **Through Your Organization**
   - Contact your Microsoft 365 admin
   - Request Fabric capacity assignment
   - Minimum F64 recommended for this notebook

**Documentation**: [Get started with Microsoft Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial)

### Step 2: Create a Workspace

1. Open [Microsoft Fabric](https://app.fabric.microsoft.com/)
2. Click **Workspaces** in the left navigation
3. Click **+ New workspace**
4. Enter a name (e.g., "Demo Data Generation")
5. Expand **Advanced** → assign to a Fabric capacity (F64+)
6. Click **Apply**

**Documentation**: [Create a workspace](https://learn.microsoft.com/fabric/get-started/create-workspaces)

### Step 3: Create a Lakehouse

1. In your workspace, click **+ New**
2. Select **Lakehouse**
3. Name it (e.g., "DemoLakehouse")
4. Click **Create**

**Documentation**: [Create a lakehouse](https://learn.microsoft.com/fabric/data-engineering/create-lakehouse)

---

## 📥 Importing the Notebook

### Method 1: Upload from Local File (Recommended)

1. Download `Fabric_Notebook_with_Generative_Data_Licensed.ipynb` from GitHub
2. In your Fabric workspace, click **+ New**
3. Select **Import notebook**
4. Click **Upload** and select the `.ipynb` file
5. Click **Open**
6. The notebook appears in your workspace

### Method 2: Import from GitHub URL

1. In your workspace, click **+ New**
2. Select **Import notebook**
3. Click **From GitHub** (if available)
4. Paste the GitHub URL to the notebook
5. Click **Import**

### Method 3: Copy/Paste (If Above Don't Work)

1. Create a **New notebook** in Fabric
2. Open the downloaded `.ipynb` file in a text editor
3. Copy the entire JSON content
4. In Fabric, switch to **Edit mode** (if not already)
5. Delete the default cell
6. Use the notebook's import JSON feature (if available)

**After Import:**

1. **Attach to your Lakehouse**
   - Click the lakehouse icon in the left panel
   - Click **Add**
   - Select your lakehouse
   - Click **Add**

2. **Verify Lakehouse Connection**
   - Look for your lakehouse name in the left panel
   - If missing, click **Add lakehouse** and select it

**Documentation**: [Import and export notebooks](https://learn.microsoft.com/fabric/data-engineering/author-execute-notebook#import-existing-notebooks)

---

## 🤖 Setting Up Azure OpenAI

### Option A: Use Preloaded Mode (No Setup Required)

If you just want to test the notebook without Azure OpenAI:

1. In the notebook, find the **Configuration** cell
2. Change `generation_mode = "AI"` to `generation_mode = "Preloaded"`
3. Run the notebook - it will use built-in themes!

### Option B: Set Up Azure OpenAI (For Custom Themes)

**Step 1: Create Azure OpenAI Resource**

1. Go to [Azure Portal](https://portal.azure.com/)
2. Click **+ Create a resource**
3. Search for **Azure OpenAI**
4. Click **Create**
5. Fill in:
   - **Subscription**: Your Azure subscription
   - **Resource group**: Create new or use existing
   - **Region**: Choose (East US, West Europe, etc.)
   - **Name**: Something memorable (e.g., "my-openai-demos")
   - **Pricing tier**: Standard S0
6. Click **Review + Create** → **Create**
7. Wait for deployment (1-2 minutes)

**Documentation**: [Create an Azure OpenAI resource](https://learn.microsoft.com/azure/ai-services/openai/how-to/create-resource)

**Step 2: Deploy a Model**

1. Once created, click **Go to resource**
2. Click **Keys and Endpoint** (save these for later!)
3. Click **Model deployments** → **Manage Deployments**
4. This opens **Azure OpenAI Studio**
5. Click **Deployments** → **+ Create new deployment**
6. Select:
   - **Model**: `gpt-4o` (recommended) or `gpt-35-turbo`
   - **Deployment name**: Something simple like `gpt4o-demo`
   - **Version**: Latest available
7. Click **Create**

**Documentation**: [Deploy a model](https://learn.microsoft.com/azure/ai-services/openai/how-to/create-resource#deploy-a-model)

**Step 3: Get Your Credentials**

You need three things:

1. **Endpoint URL**
   - In Azure Portal → your OpenAI resource → **Keys and Endpoint**
   - Copy the **Endpoint** (looks like: `https://YOUR-RESOURCE.openai.azure.com/`)

2. **API Key**
   - Same location → **Keys and Endpoint**
   - Copy **KEY 1** or **KEY 2**
   - ⚠️ **Keep this secret!**

3. **Deployment Name**
   - This is what you named your deployment (e.g., `gpt4o-demo`)

**Step 4: Add to Notebook**

In the notebook's **Configuration** cell:

```python
# Direct credentials - REMEMBER TO ROTATE AFTER DEMOS!
direct_endpoint = "https://YOUR-RESOURCE.openai.azure.com/"  # Paste your endpoint
direct_key = "YOUR_API_KEY_HERE"  # Paste your API key
direct_deployment = "gpt4o-demo"  # Your deployment name
```

**⚠️ CRITICAL SECURITY NOTE:**
- After demos, regenerate your API key in Azure Portal
- Never commit notebooks with real keys to public repositories
- For production, use Azure Key Vault or Managed Identity

**Documentation**: [Authenticate to Azure OpenAI](https://learn.microsoft.com/azure/ai-services/openai/how-to/authentication)

---

## ▶️ First Run

### Quick Start Checklist

- [ ] Notebook imported to Fabric
- [ ] Lakehouse attached
- [ ] Azure OpenAI configured (or using Preloaded mode)
- [ ] Configuration cell updated with your settings

### Running the Notebook

1. **Review Configuration** (Cell 1)
   ```python
   business_type = "Restaurant"  # Start with this
   theme = "Caveman Fine Dining"  # Fun default
   generation_mode = "AI"  # Or "Preloaded"
   record_scale = "small"  # Start small (10K records)
   ```

2. **Run All Cells**
   - Click **Run all** in the toolbar
   - Or: Press `Shift + Enter` through each cell
   - Watch for the ✅ success indicators

3. **Verify Output**
   - Check for "✅ Generation Complete!"
   - Look for your tables in the Lakehouse
   - Run the sample analytics at the end

### Expected Runtime

| Scale | Records | Typical Runtime (F64) |
|-------|---------|---------------------|
| Small | 10,000 | 2-3 minutes |
| Medium | 25,000 | 3-5 minutes |
| Large | 500,000 | 15-30 minutes |

---

## 🎨 Customization Guide

### Change Business Type

In the **Configuration** cell:

```python
business_type = "Retail"      # Products, sales, inventory
business_type = "Restaurant"  # Menu items, orders, dining
business_type = "Healthcare"  # Procedures, visits, treatments
```

**What changes:**
- Terminology (products vs menu items vs procedures)
- Price ranges (automatically adjusted)
- Fact table name (`demo_fact_sales`, `demo_fact_orders`, or `demo_fact_visits`)

### Change Theme

Any creative theme works! Examples:

```python
# Fantasy
theme = "Wizarding World Market"
theme = "Dragon's Treasure Trove"
theme = "Elvish Apothecary"

# Sci-Fi
theme = "Cyberpunk Night Market"
theme = "Martian Colony Store"
theme = "Intergalactic Trading Post"

# Historical
theme = "Medieval Blacksmith"
theme = "Victorian Tea Room"
theme = "Ancient Roman Marketplace"

# Whimsical
theme = "Underwater Bubble Cafe"
theme = "Cloud Castle Boutique"
theme = "Time Traveler's Emporium"
```

**How it works:**
- Azure OpenAI generates creative names matching your theme
- Products, locations, and brands all get themed names
- More specific themes = better results!

### Adjust Dataset Size

```python
record_scale = "small"   # 10,000 transactions - quick tests
record_scale = "medium"  # 25,000 transactions - balanced demos
record_scale = "large"   # 500,000 transactions - stress testing
```

### Change Date Range

In **Step 5: Generate Date Dimension**:

```python
# Current default: 2022-2027
start_date = datetime(2022, 1, 1)
end_date = datetime(2027, 12, 31)

# Change to whatever you need:
start_date = datetime(2020, 1, 1)  # More history
end_date = datetime(2030, 12, 31)  # Further future
```

### Modify Price Ranges

In **Step 7: Generate Product Dimension**:

```python
# Current ranges
price_ranges = {
    "Retail": (5, 500),
    "Restaurant": (8, 150),
    "Healthcare": (50, 5000)
}

# Customize for your needs:
price_ranges = {
    "Retail": (10, 1000),      # Higher-end retail
    "Restaurant": (15, 300),    # Fine dining
    "Healthcare": (100, 10000)  # Specialized procedures
}
```

### Add More Locations

In **Step 6: Generate Location Dimension**:

```python
# Current: 100 locations
for i in range(100):

# Change to more locations:
for i in range(200):  # Now generates 200 locations

# Add more cities:
us_cities = [
    {"city": "Seattle", "region": "West", "lat": 47.6062, "lon": -122.3321},
    {"city": "Miami", "region": "South", "lat": 25.7617, "lon": -80.1918},
    {"city": "Denver", "region": "West", "lat": 39.7392, "lon": -104.9903},
    # Add more cities here
]
```

---

## 🔧 Advanced Modifications

### Add a New Business Type

**1. Update Configuration Options**

In **Step 1**, add your new type:

```python
# Business Type: "Retail", "Restaurant", "Healthcare", "Education"
business_type = "Education"
```

**2. Add Terminology (Step 4)**

```python
elif business_type == "Education":
    product_term = "courses and programs"
    service_term = "educational services"
```

**3. Add Price Range (Step 7)**

```python
price_ranges = {
    "Retail": (5, 500),
    "Restaurant": (8, 150),
    "Healthcare": (50, 5000),
    "Education": (200, 5000)  # Course prices
}
```

**4. Add Fact Table Name (Step 8)**

```python
fact_table_names = {
    "Retail": "demo_fact_sales",
    "Restaurant": "demo_fact_orders",
    "Healthcare": "demo_fact_visits",
    "Education": "demo_fact_enrollments"
}
```

### Add Custom Dimension Tables

Want to add a customer dimension? Here's how:

**After Step 7 (Product Dimension), add a new cell:**

```python
print("\n" + "="*70)
print("👥 Generating Customer Dimension\n")

customer_data = []
for i in range(1000):  # 1000 customers
    customer_data.append({
        'customer_key': i + 1,
        'first_name': random.choice(theme_first_names),
        'last_name': random.choice(theme_last_names),
        'email': f"customer{i+1}@example.com",
        'signup_date': random.choice(date_keys),
        'customer_tier': random.choice(['Bronze', 'Silver', 'Gold', 'Platinum'])
    })

df_customer = pd.DataFrame(customer_data)
dim_customer = spark.createDataFrame(df_customer)

print(f"✅ Generated {dim_customer.count()} customers")
```

**Then in Step 8, add customer_key to fact table:**

```python
@F.udf(returnType=IntegerType())
def random_customer_key():
    return random.randint(1, 1000)

fact_df = spark.range(fact_row_count) \
    .withColumn(f"{fact_name.split('_')[-1]}_key", F.monotonically_increasing_id() + 1) \
    .withColumn("date_key", random_date_key()) \
    .withColumn("location_key", random_location_key()) \
    .withColumn("product_key", random_product_key()) \
    .withColumn("customer_key", random_customer_key())  # NEW!
    .withColumn("quantity", random_quantity())
```

**And in Step 9, write the new dimension:**

```python
print("   Writing demo_dim_customer...")
dim_customer.write.format("delta").mode("overwrite").saveAsTable("demo_dim_customer")
print(f"      ✅ {dim_customer.count():,} rows\n")
```

### Add Pre-loaded Themes

In **Step 4**, add your theme to the `preloaded_data` dictionary:

```python
"Your New Theme": {
    "product_names": ["Product1", "Product2", ...],  # 20+ items
    "categories": ["Cat1", "Cat2", ...],              # 8 items
    "brands": ["Brand1", "Brand2", ...],              # 8 items
    "locations": ["Location1", "Location2", ...],     # 10+ items
    "services": ["Service1", "Service2", ...],        # 6 items
    "adjectives": ["Adj1", "Adj2", ...],              # 8 items
    "first_names": ["Name1", "Name2", ...],           # 12+ items
    "last_names": ["Last1", "Last2", ...]             # 10+ items
}
```

### Optimize for Larger Datasets

For 1M+ records, add these optimizations:

**Step 2: Spark Configuration**

```python
# Increase partitions for large datasets
spark.conf.set("spark.sql.shuffle.partitions", "16")  # Default is 8
spark.conf.set("spark.sql.adaptive.enabled", "true")
spark.conf.set("spark.sql.adaptive.coalescePartitions.enabled", "true")
```

**Step 9: Add Z-Ordering**

```python
# After writing fact table
spark.sql(f"OPTIMIZE {fact_name} ZORDER BY (date_key, location_key)")
print("   ✅ Optimized with Z-ordering")
```

### Change Azure OpenAI Model

In **Step 4**, adjust model parameters:

```python
data = {
    "messages": [...],
    "temperature": 0.9,      # Higher = more creative (0.7-1.0)
    "max_tokens": 16000,     # Increase for larger lists
    "top_p": 0.95,           # Nucleus sampling
    "frequency_penalty": 0.5  # Reduce repetition
}
```

### Add Data Quality Constraints

Want to ensure certain products are always in stock? Add logic in Step 7:

```python
# Flag popular products
product_data.append({
    'product_key': i + 1,
    'product_name': product_name,
    'category': category,
    'is_featured': True if i < 10 else False,  # First 10 are featured
    'min_stock': 100 if i < 10 else 10,        # Higher min stock for featured
    # ... rest of fields
})
```

---

## 🐛 Troubleshooting

### Notebook Won't Import

**Issue**: Import fails or notebook is corrupted

**Solutions**:
1. Verify the file is a valid `.ipynb` file
2. Try opening in [JupyterLab](https://jupyter.org/) first to validate
3. Check file size isn't too large (should be <1MB)
4. Use copy/paste method as fallback

### Can't Attach Lakehouse

**Issue**: Lakehouse doesn't appear or won't attach

**Solutions**:
1. Refresh the workspace
2. Verify the lakehouse exists in the same workspace
3. Check you have permissions on the lakehouse
4. Try removing and re-adding the lakehouse

### Azure OpenAI Returns 401 Error

**Issue**: Authentication failed

**Solutions**:
1. Double-check endpoint URL (must end with `/`)
2. Verify API key is correct (copy/paste carefully)
3. Check key hasn't been regenerated in Azure Portal
4. Ensure deployment name matches exactly

### "Unterminated String" Error

**Issue**: JSON response is truncated

**Solutions**:
1. Increase `max_tokens` to 8000 or 16000
2. Reduce the number of items requested (Step 4 prompt)
3. Try a different API version
4. Use Preloaded mode as fallback

### Tables Don't Appear in Lakehouse

**Issue**: Notebook runs but tables aren't visible

**Solutions**:
1. Refresh the Lakehouse explorer (right-click → Refresh)
2. Check you're looking in the **Tables** folder, not Files
3. Run the refresh cell (Step 9 optional cell)
4. Verify write operations completed without errors

### Slow Performance

**Issue**: Notebook takes too long to run

**Solutions**:
1. Start with `record_scale = "small"`
2. Check Fabric capacity isn't overloaded
3. Reduce number of shuffle partitions for small datasets
4. Run during off-peak hours

### Out of Memory

**Issue**: Spark runs out of memory

**Solutions**:
1. Reduce `record_scale`
2. Increase Fabric capacity tier
3. Add `.cache()` sparingly, not everywhere
4. Process in smaller batches

---

## 📚 Additional Resources

### Microsoft Fabric
- [Official Documentation](https://learn.microsoft.com/fabric/)
- [Fabric YouTube Channel](https://www.youtube.com/@MicrosoftFabric)
- [Fabric Community](https://community.fabric.microsoft.com/)

### Azure OpenAI
- [Quickstart Guide](https://learn.microsoft.com/azure/ai-services/openai/quickstart)
- [API Reference](https://learn.microsoft.com/azure/ai-services/openai/reference)
- [Pricing Calculator](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)

### Power BI & Visualization
- [Power BI with Fabric](https://learn.microsoft.com/power-bi/fundamentals/fabric-get-started)
- [Direct Lake Mode](https://learn.microsoft.com/power-bi/enterprise/directlake-overview)

### PySpark
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Delta Lake Guide](https://docs.delta.io/)

---

## 💡 Tips & Best Practices

### For Demonstrations
1. **Pre-generate data** before your presentation
2. **Use memorable themes** that match your audience
3. **Start with small scale** to show speed
4. **Have Preloaded mode** ready as backup
5. **Regenerate API keys** immediately after

### For Learning
1. **Read the comments** in each cell carefully
2. **Experiment with themes** - it's fun!
3. **Try different business types** to see variations
4. **Modify one thing at a time** when customizing
5. **Save working versions** before major changes

### For Production Use
1. **Use Key Vault** for credentials
2. **Implement error handling** around API calls
3. **Add logging** for debugging
4. **Version control** your modifications
5. **Document** your customizations

---

## 🎉 You're Ready!

You now have everything you need to:
- ✅ Set up Microsoft Fabric
- ✅ Configure Azure OpenAI
- ✅ Import and run the notebook
- ✅ Customize for your needs
- ✅ Troubleshoot common issues

**Happy data generating!** 🚀

If you have questions or run into issues, check the [main README](README.md) or open an issue on GitHub.

---

*Last Updated: November 2025*
