# TinyMCE-Local-image-save
TinyMCE Local Save

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
