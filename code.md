#nodejs
##singleton

```
var singleton = (function () {
this.dateValue = "1234";
 this.imageStorePath = "./assets/images";
return this;
})();
module.exports = singleton;
```

##app.js

```
const multer = require("multer");
var Storage = multer.diskStorage({
destination: function(req, file, callback) {
callback(null, singleton.imageStorePath);
 },
 filename: function(req, file, callback) {
callback(null, "img" + "_" + singleton.dateValue + "_" + file.originalname);
}
});
var upload = multer({
storage: Storage
}).single("myImage");

        app.post('/api/imageUploadSessionAdd', function(req, res){
        var model = req.body.modelName;
                singleton.dateValue = req.body.date;
                if(req.body.type =="EditImage"){
                      singleton.imageStorePath = "./assets/images/EditImages";
                }else{
                   singleton.imageStorePath = "./assets/images/" + model.split(' ').join(' ');
                }
                return res.send({data: "Session Added."+singleton.imageStorePath});
        });

        app.post("/api/imageUpload", function(req, res) {
        upload(req, res, function(err) {
        var date = req.body.date;
                var title = req.body.title;
                var modelId = req.body.modelId;
                var modelNm = req.body.modelName;
                var imageName = req.body.imageName;
                if (err) {
        return res.end("Something went wrong!");
        } else{
        console.log('upload images')
        }
        });
        });

```

##reactweb

```
<input
                            type="file"
                            className="form-control"
                            name="myImage"
                            onChange={this.onChange}
                          />

                          onChange(e) {
    this.setState({
      file: e.target.files[0],
      imageName: e.target.files[0].name
    });

}

onFormSubmit(e: any) {
e.preventDefault();
let optionValue = JSON.parse(e.target.option.value);
console.log({ optionValue });

const formData = new FormData();
formData.append("myImage", this.state.file);
formData.append("date", utils.getUnixTimeDate(new Date()));
formData.append("title", e.target.title.value);
formData.append("modelId", optionValue.id);
formData.append("modelName", optionValue.modelName);
formData.append("imageName", this.state.imageName);

var body = {
myImage: this.state.file,
date: utils.getUnixTimeDate(new Date()),
modelName: optionValue.modelName,
imageName: this.state.imageName
};
axios
.post(apiary.imageUploadSessionAdd, body)
.then(response => {
let data = response.data;
console.log({ data });
})
.catch(error => {});

const config = {
headers: {
"content-type": "multipart/form-data"
}
};
axios
.post(apiary.imageUpload, formData, config)
.then(response => {
alert(response.data);
})
.catch(error1 => {});
}
```
