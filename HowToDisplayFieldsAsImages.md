# How to display model fields as images #

If you have images stored in either blob fields or external file system directories, you can have Kitto display thumbnails in the form and allow the user to upload and download images. Look [here](HowToUploadFilesToTheDatabase.md) for details about defining the fields.

Once you have defined your `Blob` or `FileReference` field, just add as `IsPicture` node to trigger The picture-specific display and behaviour:

```
Fields:
  ...
  Picture: Blob
    ...
    IsPicture: True
      Thumbnail:
        # Default is 100 for both dimensions.
        Width: 150
        Height: 150
```

You can use the `Thumbnail` subnode to set the size (in pixels) that the image will occupy on a form. Don't forget that about 10 pixels will be added to each dimension for the frame, and that on the right of the image Kitto will need to display a small toolbar with Download, Upload and Clear buttons (the latter two only as long as the form is not read-only).

The example above uses a blob field, but it works the same way for file references.

---

Currently, Kitto is only capable of generating thumbnails for jpg and png images. Other formats are either not supported (f. ex. tiff) or sent to the browser in full size (f. ex. bmp).

---

If the actual image is smaller that the thumbnail size you set, it is sent to the browser untouched and no thumbnail is generated (which saves resources).