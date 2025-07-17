import os
from faker import Faker
from docx import Document
from datetime import datetime
import random

fake = Faker()

# Example: Extend these as needed from instructions or API params
DEFAULT_INSTRUCTION_OVERRIDES = {}

# Synth ingredients for demo purposes if not provided
DEFAULT_INGREDIENTS = [
    "Dried Strawberries", "Apple Fiber", "Pea Protein Isolate", "Carrot Powder",
    "Citrus Pectin", "Freeze-dried Raspberries", "Coconut Flour", "Skimmed Milk Powder"
]

def generate_rmq_docx(
    ingredient_name: str,
    out_path: str,
    custom_instructions: dict = None
):
    """
    Generates a new synthetic raw material questionnaire DOCX file at the requested location.

    :param ingredient_name: The main raw material/ingredient for the form.
    :param out_path: Where to save the .docx file.
    :param custom_instructions: dict of specific field overrides (optional)
    """
    ci = custom_instructions or DEFAULT_INSTRUCTION_OVERRIDES

    doc = Document()
    today = datetime.now().strftime("%d.%m.%Y")
    code = fake.unique.bothify('RMQ###')
    sap_no = fake.unique.bothify('3######')
    supplier = fake.company() + " GmbH"
    supplier_addr = f"{fake.country()} {fake.postcode()} {fake.city()} {fake.street_address()}"
    batch_date = today

    # HEADER
    doc.add_heading('Raw material questionnaire', 0)
    doc.add_paragraph(f"Re.:")
    doc.add_paragraph(f"Index:")
    doc.add_paragraph(f"Page: QM-D-490-12-01-FO-01  0  1 of 17")
    
    # Meta
    doc.add_paragraph("")
    doc.add_paragraph("created / modified by\tchecked & approved by")
    doc.add_paragraph(f"{fake.name()}\tRegulatory Affairs")
    doc.add_paragraph(f"{fake.name()}\tQM Germany")
    doc.add_paragraph(today)
    doc.add_paragraph("")

    doc.add_paragraph("The following information are to be completed by the supplier:")
    doc.add_paragraph(f"Supplier article name: {ci.get('supplier_article_name', ingredient_name)}")
    doc.add_paragraph(f"Supplier article code: {ci.get('code', code)}")
    doc.add_paragraph(f"HARIBO-SAP-number and related material short description: {ci.get('sap_no', sap_no)} {ingredient_name} {ci.get('code', code)}")
    doc.add_paragraph("")
    doc.add_paragraph(f"Supplier: {ci.get('supplier', supplier)}")
    doc.add_paragraph(f"Address: {ci.get('supplier_addr', supplier_addr)}")
    doc.add_paragraph(f"Contact person (Sales): {fake.name()}")
    doc.add_paragraph(f"Technical support: {fake.name()}")
    doc.add_paragraph(f"QA/QM: {fake.name()}")
    doc.add_paragraph(f"Crisis phone number (24/7): {fake.phone_number()}")
    doc.add_paragraph(f"Manufacturer: {fake.company()} Manufacturing Inc.")
    doc.add_paragraph("")

    # SECTION 1
    doc.add_heading("1. Product specification", level=1)
    doc.add_paragraph(f"name of the raw material: {ingredient_name}")
    doc.add_paragraph(f"product description: {fake.color_name()} colored, {fake.word()} texture, mild aroma, typical flavor.")
    doc.add_paragraph("process description: Cleaned, processed, and packed under hygienic conditions (synthetic).")
    doc.add_paragraph(f"chemical/physical: moisture {random.randint(5,15)}%, pH {random.uniform(3,7):.1f}")
    doc.add_paragraph("microbiology: TVC ≤ 1e4 CFU/g, Yeasts ≤ 200 CFU/g, Moulds ≤ 100 CFU/g")
    doc.add_paragraph("usage amount recommendation: Up to 10% of finished product weight.")
    doc.add_paragraph("special labeling requirements: None.")

    # SECTION 2 (Example ingredients list)
    doc.add_heading("2. Ingredients", level=1)
    ing1 = ingredient_name
    percent1 = random.randint(90,99)
    ing2 = fake.word().capitalize()
    percent2 = 100 - percent1
    doc.add_paragraph(" | Ingredient          | Function           | %   | Origin             | Geo. Origin |")
    doc.add_paragraph(f" | {ing1:<18} | Main               | {percent1:<3} | {fake.word().capitalize()}         | {fake.country()} |")
    doc.add_paragraph(f" | {ing2:<18} | Carrier/Additive   | {percent2:<3} | {fake.word().capitalize()}         | {fake.country()} |")
    doc.add_paragraph("...")

    # Further sections (add more as needed)
    doc.add_heading("3. Confirmation of obligatory requirements", level=1)
    doc.add_paragraph("We agree to communicate any changes pertinent to the raw material immediately.")
    doc.add_paragraph("No hydrogenated fats/oils, nanomaterials, or coconut ingredients used. Complies with relevant EU food law.")

    doc.add_heading("4. Novel Food", level=1)
    doc.add_paragraph("Is the raw material a novel food according to (EU) 2015/2283?  No.")

    doc.add_heading("5. Flavourings", level=1)
    doc.add_paragraph("No added flavourings.")

    doc.add_heading("6. Colours", level=1)
    doc.add_paragraph("No added colours (or as per ingredient type)")

    doc.add_heading("7. Colouring foods", level=1)
    doc.add_paragraph("None included.")

    doc.add_heading("8. GMO", level=1)
    doc.add_paragraph("No GMO ingredients used. 'Ohne Gentechnik' declaration possible.")

    doc.add_heading("9. Allergens", level=1)
    doc.add_paragraph("Allergen status: None or as per specification.")

    doc.add_heading("10. Vegetarian/Vegan", level=1)
    doc.add_paragraph("Vegetarian: Yes\nVegan: Yes")

    doc.add_heading("11. Halal/Kosher", level=1)
    doc.add_paragraph("Halal certificate: No\nKosher certificate: No")

    doc.add_heading("12. Palm oil", level=1)
    doc.add_paragraph("Palm oil or derivatives: Not used.")

    doc.add_heading("13. Admissibility and declaration", level=1)
    doc.add_paragraph("The product meets EU/national food regulations.")

    doc.add_heading("14. Nutrition declaration", level=1)
    doc.add_paragraph(f"Energy: {random.randint(100,400)} kcal/100g\nProtein: {random.randint(1,20)}g/100g\nFat: {random.randint(0,10)}g/100g\nCarbohydrate: {random.randint(10,90)}g/100g")

    doc.add_heading("15. Occupational safety and hazardous substances", level=1)
    doc.add_paragraph("No hazards. Food grade material.")

    doc.add_heading("16. Other", level=1)
    doc.add_paragraph("No further remarks.")

    out_dir = os.path.dirname(out_path)
    if out_dir and not os.path.exists(out_dir):
        os.makedirs(out_dir)
    doc.save(out_path)
    return out_path

# If run as main: generate a demo file
if __name__ == "__main__":
    outpath = "rmq_sample_Output.docx"
    print(f"Generating synthetic RMQ DOCX: {outpath}")
    path = generate_rmq_docx("Demo Ingredient", outpath)
    print(f"File written to {path}")
