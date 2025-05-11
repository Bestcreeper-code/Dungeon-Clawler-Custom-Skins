### **Skin Folder** Structure
*Skin_name (folder name that gets displayed in skin selector)*  
├── *Skin_name* **config**.txt  
├── *Skin_name* **attack**.png  (Size: depends on image size)  
├── *Skin_name* **buff**.png  (Size: depends on image size)  
├── *Skin_name* **hit**.png  (Size: depends on image size)  
└── *Skin_name* **idle**.png  (Size: depends on image size)

(bold = required)
(*Skin_name* = Your Skin Name)

**Important: PNG file sizes are set by you.**
### **config.txt** Structure

Name: *Skin_name*  
Description: *Skin_desc*  
PerkDesc: *Skin_perk_desc*  
Items: [*Item_1* **:** *Amount_1* **;** *Item_2* **:** *Amount_2* **;** *Item_3* **:** *Amount_3* **;** ...]  
Perks: [*Perk_1* **:** *Amount_1* **;** *Perk_2* **:** *Amount_2* **;** ...]

**Important: Items and Perks are case sensitive (look in the provided Items.txt for the full list of items and perks).**
**Important: you can add a '&' after an item name to change it's material (Itemname&Material:Amount).**