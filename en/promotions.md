## Promotions ##

To promote certain articles, categories or manufacturers, OXID comes with the ability to create promotions by selecting `Customer info->promotions`. Promotions apply to already existing filters.  

To create fluently working promotions, consider the following information:  
- Please enter category and attribute names exactly as they were created in the database as the spelling is case sensitive.  
- If promotions consist of several attributes, no space may be present between the diwider and attributes. The syntax is always `attribute_1,attribute_2`.  
__Note: While a promotion can consist of several attributes, they can never contain more than one category!__  
- If your promotions contain dynamic categories, these must be addressed witz the category ID. You can get the IDs by selecting the link and using tools like firebug or developer.  
- If attributes require more than one value, "and" is the connecting operator to be used.  
