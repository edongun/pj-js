# pj-js
qwer.js

var VISION_API_KEY = "AIzaSyD2KdEtfMwgebgQbE-N9xMoU23zkzpIgVg";
var CV_URL = 'https://vision.googleapis.com/v1/images:annotate?key=' + VISION_API_KEY;

imagestring = null


function processFile (event) {
    var content = event.target.result;
    imagestring = content.replace('data:image/jpeg;base64,', '');
   
  }

function uploadFiles(files){
    //var file = $('#fileform [name=fileField]')[0].files[0];
    var file = files[0];
    var reader = new FileReader();
    reader.onloadend = processFile

   
    reader.readAsDataURL(file);

    // console.log(reader)
  }

function Send() {
    var request = {
        requests: [{
          image: {
            content: imagestring
          },
          features: [{
            type: $('#fileform [name=type]').val(), // 'LABEL_DETECTION',
            maxResults: 200
          }]
        }]
      };

 
    $.ajax({
        type: "POST",
        url: CV_URL,
        headers : {
            "Accept" : "application/json",
        "Content-Type" :  "application/json" },
        data: JSON.stringify(request),
        contentType: "application/json; charset=utf-8"
    }).done(displayJSON)
    .fail(function (error) {
        alert("!/error  js에서 에러발생: " + error);
    });
}

function displayJSON (data) {
    var contents = JSON.stringify(data, null, 4);
    $('#results').text(contents);

    // label detect인 경우만 해당됨

    dlabels = ""
    labels = data.responses[0].labelAnnotations
    console.log(labels)
    labels.forEach(function(label){
        dlabels += label.description + "\n"
    })
    $('#resultr').text(dlabels);
  }

  ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
chat.js

var OPENAI_API_KEY =
'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';

function Send(){

  var sQuestion = txtMsg.value;
  var data = {
    model: "text-davinci-003",
    prompt: sQuestion,
    max_tokens: 2048,
    temperature: 0
}  
$.ajax({
  type: "POST",
  url: 'https://api.openai.com/v1/completions',
  headers:{
    "Accept" : "application/json",
    "Content-Type": "application/json", 
    "Authorization": "Bearer " +  OPENAI_API_KEY },
  data: JSON.stringify(data),

}).done(function(response) {


     var sanswer = response.choices[0].text
     txtOut.value = sanswer

}).fail(function(error) {
  alert("!/js/user.js에서 에러발생: " + error.statusText);
  console.log(error)
});
}

function Draw(){

  var sQuestion = imgMsg.value;
  var data = {
    prompt: sQuestion,
    n:2,
    size: "512x512"
}  
$.ajax({
  type: "POST",
  url: 'https://api.openai.com/v1/images/generations',
  headers:{
    "Accept" : "application/json",
    "Content-Type": "application/json", 
    "Authorization": "Bearer " +  OPENAI_API_KEY },
  data: JSON.stringify(data),

}).done(function(response) {

      gimage.src = response.data[0].url
      gimage2.src = response.data[0].url

}).fail(function(error) {
  alert("!/js/user.js에서 에러발생: " + error.statusText);
  console.log(error)
});
}


        let ChatObject = {
                getAnswer: function() {
            
                  alert("$.ajax 실행")
                  var data = {
                        model: "text-davinci-003",
                        prompt: "가을에 듣기 좋은 음악이 뭘까",
                        max_tokens: 2048,
                        temperature: 0
                  }  
                  $.ajax({
                    type: "POST",
                    url: 'https://api.openai.com/v1/completions',
                    headers:{
                        "Accept" : "application/json",
                        "Content-Type": "application/json", 
                        "Authorization": "Bearer " +  OPENAI_API_KEY },
                    data: JSON.stringify(data),
            
                  }).done(function(response) {
            
                         console.log(response)
                         alert(response.choices[0].text)
            
                  }).fail(function(error) {
                    alert("!/js/user.js에서 에러발생: " + error.statusText);
                    console.log(error)
                  });
                },
               }
              
              //  ChatObject.getAnswer();

              

  
