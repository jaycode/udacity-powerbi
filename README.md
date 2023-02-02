# Udacity Power BI project - Create a Data Model for Seven Sages Company

This is my submission for the project mentioned above.

## Notes and Troubleshoots

### Problem when creating Monthly Sales Logs

To create Monthly Sales Logs, I loaded a folder instead of loading the Sales sheets one by one. This had presented a problem due to the sheet names being different in each file.

To work around this, I followed [this tutorial from radacad](https://radacad.com/get-data-from-multiple-excel-files-with-different-sheet-names-into-power-bi) that goes as follows:

1. Use the "Combine Files" feature. Choose any file there (I picked the Aug 2021 spreadsheet since it was the first file that does not have an extra column "column6" in its table) and choose the "Sheet" object.
2. After combining the files, the "Monthly Sales Logs" query created a table that contained errors on rows other than the one coming from the Aug 2021 spreadsheet.
3. Open the "Transform Sample File" query that was automatically created.
4. Click on "Navigation" from the "Applied Steps" query setting.
5. Change the function from `Source{[Item="Aug 2021 Sales",Kind="Sheet"]}` to `Source{0}`. This will get always the first sheet in each spreadsheet.

Combine from Folder lesson: Bigger Transformations & Date Tables > [Bigger Transformations Across Queries](https://learn.udacity.com/nanodegrees/nd045-ent-anglo/parts/cd0012/lessons/396323d3-bf4e-4b12-8185-aad467f6ffdf/concepts/07b6b1b7-ff3f-44c0-9880-66d21aa9c002) or [this video on Youtube](https://www.youtube.com/watch?v=LeiUHV64-z4).

### Monthly Sales Logs - Incorrect column name for "Currency" in `Monthly Sales Logs/SSBC - May 2021 Sales.xlsx`.

The idea here is we want to use column orders instead of column names for inserting rows.

1. Open the "Transform Sample File" query.
2. Remove the "Promoted Headers" step. By doing this, the column names are going to be a part of the rows.
3. Update the data type of the 3rd column (Date) to Date object. This will replace the cells with the value "Date" to ERROR objects.
4. Remove errors.
5. Rename the columns manually.
6. Also, correct the data type of the "Qty" column.

### Cost of Sales Wrong Total

The total when using Sum for Sales ($USD) was 238,161 which is incorrect since it multiplies the sum of Sales Price and the sum of Qty. This issue is covered in "Relationships and Relationship-Related DAX" > "[Explicit Measures with DAX](https://learn.udacity.com/nanodegrees/nd104-ent-anglo/parts/cd0012/lessons/9c3fbe2c-0273-4dd3-88e2-873d3ab4406a/concepts/30ec835d-5b97-4df4-b523-f3f59b28b1f8)" or [this video on Youtube](https://www.youtube.com/watch?v=Whho8aTeo4g).

As recommended in the video, I rewrote the code to this:

```
Sales ($USD) =
SUMX('fact_sales', [Qty] * RELATED('dim_products'[Per Unit Sales price]))
```

and do the same thing for Cost of Sales ($USD) and Sales ($CAD).
