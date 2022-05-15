# TinyMCE Local image save for .net core 5.0

Add directory wwwroot/images


LocalSave Controller
```c#
 public class LocalSave : Controller
    {
        [HttpGet("SaveTest")]
        public ActionResult SaveTest()
        {
            return View();
        }

        [HttpPost("FileUpload")]
        public async Task<IActionResult> FileUpload(IFormFile file)do not change. TinyMCE only accept "file"
        {
            var random = "";
            var name = file.FileName.ToLower();
            if (file.Length > 0)
            {
                random = Guid.NewGuid() + name;//create a new name
                var filePath = Path.Combine("wwwroot/images" , random);/filePath

                using (var stream = new FileStream(filePath, FileMode.Create))
                {
                    await file.CopyToAsync(stream);
                }
            }
            return Ok(new {location = random});//json data correct for TinyMCE
        }
    }
``` 


SaveTest.cshtml

```html
<form method="post" enctype="multipart/form-data" asp-controller="LocalSave" asp-action="FileUpload">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload file:</p>
            <input type="file" name="file" multiple />//file name important
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

Add in Layout TinyMCE Scripts 

```javascript
<head>
  <script src="https://cdn.tiny.cloud/1/YOUR-APİ/tinymce/6/tinymce.min.js" referrerpolicy="origin"></script> //Change api key
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
                images_upload_base_path:'\\images',
                });
   </script>
</head>
```


TinyMCE Html add in form

```html

<form asp-action="Add">//change your ActionResult
  <div class="form-group">
       <label asp-for="Your-obje" class="control-label"></label>
       <textarea asp-for="Your-obje" id="mytextarea" class="form-control"></textarea> //The field you want to record in your models
       <span asp-validation-for="Your-obje" class="text-danger"></span>
  </div>
  <div class="form-group">
      <input type="submit" value="Save" class="btn btn-primary"/>
  </div>
</form>
```
