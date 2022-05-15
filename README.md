# TinyMCE Local image save for .net core 5.0

Local Save Controller
```c#
 public class LocalSave : Controller
    {
        [HttpGet("SaveTest")]
        public ActionResult SaveTest()
        {
            return View();
        }

        [HttpPost("FileUpload")]
        public async Task<IActionResult> FileUpload(IFormFile file)
        {
            var random = "";
            var name = file.FileName.ToLower();
            if (file.Length > 0)
            {
                random = Guid.NewGuid() + name;
                var filePath = Path.Combine("wwwroot/Images" , random);

                using (var stream = new FileStream(filePath, FileMode.Create))
                {
                    await file.CopyToAsync(stream);
                }
            }
            return Ok(new {location = random});
        }
    }
``` 


SaveTest.cshtml

```html
<form method="post" enctype="multipart/form-data" asp-controller="Photo" asp-action="FileUpload">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload file:</p>
            <input type="file" name="file" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

Add TinyMCE Scripts

```javascript
<head>
  <script src="https://cdn.tiny.cloud/1/YOUR-APİ/tinymce/6/tinymce.min.js" referrerpolicy="origin"></script>
  <script>  tinymce.init({
                selector: '#mytextarea',
                height : "650",
                plugins: 'code emoticons imagetools preview print autosave save image link media table anchor lists checklist wordcount imagetools paste ',
                toolbar: 'undo redo | formatselect | ' +
                  'bold italic backcolor | alignleft aligncenter ' +
                  'alignright alignjustify | bullist numlist outdent indent | ' +
                  'removeformat | help'+'|code|'+ '| link image| ',
                images_upload_url: '/FileUpload',
                automatic_uploads: true,
                images_upload_base_path:'\\Images',
                });
   </script>
</head>
```
