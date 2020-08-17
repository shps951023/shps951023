Today answer a S.O question : [c# - How can I compress a Directory then return the resulting byte array, without physically creating zip file on disk? - Stack Overflow](https://stackoverflow.com/questions/59762713/how-can-i-compress-a-directory-then-return-the-resulting-byte-array-without-phy/59762998#59762998)  
and sharing how C# user can use [DotNetZip Library](https://github.com/haf/DotNetZip.Semverd) to make zip download api without creating any file on server. 

Controller : 
```C#
using System;
using System.Web.Mvc;
using System.Collections.Generic;
using Ionic.Zip;
using System.IO;

namespace ZipDemo
{
    public class HomeController : Controller
    {
        [HttpGet]
        public ActionResult Index()
        {
            return View();
        }

        [HttpPost]
        public FileResult Download()
        {
            var bytes = default(byte[]);
            using (var zip = new ZipFile())
            {
                zip.AddEntry("text.txt", new byte[] {});
                zip.AddEntry("text\\text.txt", new byte[] {});
                
                using (var ms = new MemoryStream())
                {
                    zip.Save(ms);
                    bytes = ms.ToArray();
                } 
            }       
            return File(bytes, System.Net.Mime.MediaTypeNames.Application.Zip);
        }
    }
}
```

View : 
```html
@{
	Layout = null;
}

<!DOCTYPE html>
<html lang="en">
	<head>
		<!-- CSS Includes -->
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">	
	</head>
	<body>
		<div class="container">
			<div class="col-md-6 col-md-offset-3">
				<h1>Download ZIP</h1>
				@using (Html.BeginForm("Download","Home"))
				{

					<input type="submit" class="btn btn-success submit" />
				}
			</div>
		</div>
	</body>
</html>

```

[Online Demo Link ZipDoNet_MemoryStream | C# Online Compiler | .NET Fiddle](https://dotnetfiddle.net/OahvFS)
![](https://i.imgur.com/2kVTPML.png)
![](https://i.imgur.com/v4KkCu4.png)
