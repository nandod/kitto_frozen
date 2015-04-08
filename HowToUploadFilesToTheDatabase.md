## File upload ##

There are two data types in Kitto that will allow a user to upload files to the server. The two data types differ in the storage medium: You use `Blob` if you want to store the data in a blob field of the model's underlying table, and `FileReference` if you want the file to be stored outside the database, in a file system directory of your choice. The GUI that Kitto generates is identical in the two cases, and features a read-only description of the contents plus buttons to upload, download and clear the field.

In order to use the `Blob` data type, you need a blob column in the underlying database table. A `FileReference(N)` data type maps onto a string column (usually varchar) of length N, which will store the file name (without a path). Examples:

A blob field.
```
Fields:
  ...
  Picture: Blob
    Hint: Select a picture
    # Size constraint for uploads. You can use KB, MB and GB
    # suffixes for convenience.
    MaxUploadSize: 100KB
    # Predefined file name for downloads. If not specified,
    # it is inferred by the model's CaptionField.
    DefaultFileName: test.gif
    # Assumes the data is a picture and uses a different
    # display GUI with a thumbnail generated on the fly.
    # Otherwise a plain text description is displayed.
    IsPicture: True
      Thumbnail:
        Width: 150
        Height: 150
```

A file reference field.
```
Fields:
  ...
  Picture_File: FileReference(150)
    # By default, Kitto generates short file names and does
    # not store the path in each record, so you can use a
    # shorter field if you want.
    DisplayWidth: 40
    # This is mandatory and indicates where to store the files
    # whose automatically generated names are stored in the
    # field.
    Path: %Config:BlobPath%
    # Assumes the data is a picture and uses a different
    # display GUI with a thumbnail generated on the fly.
    # Otherwise a plain text description is displayed.
    IsPicture: True
      Thumbnail:
        Width: 150
        Height: 150
```

In both cases, you can store the uploaded file name in a separate field of the same model, ad needed, by specifying its name in the `FileNameField` attribute:

```
Fields:
  ...
  Picture_FileName: String(100)
  ...
  Picture_File: FileReference(150)
    ...
    FileNameField: Picture_File_Name
    ...
```