function sketchProc(processing) {
	// Override draw function, by default it will be called 60 times per second
  var width = 400;
  var height = 490;
  var padding = 30;
  var line_height = 24;
  var text_size = line_height * .8
  var font = processing.createFont("Courier New", text_size);
  var words = new Array();

  var shadow_offset = 0;

  var highlighted_line = 0;
  var highlighted_index = 0;

  var shift_down = false;

  var word_prefix = "";

  processing.setup = function() {
  	processing.size(width, height);
  	processing.textSize(text_size);
	processing.textFont(font);
  	processing.fill(0,0,255, 1); //transparency
  }

  processing.draw = function() {
  	processing.background(0,0,255,0);

  	//for loop and display words on different lines //TODO:
  	var current_line = 0;
  	var current_index = 0;
  	var offset = 0;
  	for (i=0;i<words.length;i++) {
  		if(offset < width-processing.textWidth(words[i])) {
  			//shadow
  			processing.fill(0,0,0);
  			for(j=0;j<shadow_offset;j++) {
  				processing.text(words[i], offset+j+1, line_height*(current_line+1)+j+1);
  			}

        if(word_prefix.length > 0) {
          if(words[i].substring(0,word_prefix.length)==word_prefix) {
            processing.fill(0,0,0);
            processing.text(words[i], offset, line_height*(current_line+1));
            offset+=processing.textWidth(words[i]);
            offset+=padding;
          }
        } else {
          processing.fill(0,0,0);
          processing.text(words[i], offset, line_height*(current_line+1));
          offset+=processing.textWidth(words[i]);
          offset+=padding;
        }
  			current_index++;
  		} else {
  			current_line++;
  			offset = 0;
  			current_index = 0;
  		}
  	}

  	/*
    // determine center and max clock arm length
    var centerX = processing.width / 2, centerY = processing.height / 2;
    var maxArmLength = Math.min(centerX, centerY);

    function drawArm(position, lengthScale, weight) {
      processing.strokeWeight(weight);
      processing.line(centerX, centerY,
        centerX + Math.sin(position * 2 * Math.PI) * lengthScale * maxArmLength,
        centerY - Math.cos(position * 2 * Math.PI) * lengthScale * maxArmLength);
    }

    // erase background
    processing.background(0,0,255, 1);

    var now = new Date();

    // Moving hours arm by small increments
    var hoursPosition = (now.getHours() % 12 + now.getMinutes() / 60) / 12;
    drawArm(hoursPosition, 0.5, 5);

    // Moving minutes arm by small increments
    var minutesPosition = (now.getMinutes() + now.getSeconds() / 60) / 60;
    drawArm(minutesPosition, 0.80, 3);

    // Moving hour arm by second increments
    var secondsPosition = now.getSeconds() / 60;
    drawArm(secondsPosition, 0.90, 1);
    */
  };

  processing.keyPressed = function() {
  	// 16 SHIFT
  	// 38 UP
  	// 37 LEFT
  	// 40 DOWN
  	// 39 RIGHT

  	if(processing.keyCode == 16) {
  		shift_down = true;
  	}

  	if(shift_down) {
		if(processing.keyCode == 38) { // UP
			if(highlighted_line > 0) {
				highlighted_line--;
			}
		} else if (processing.keyCode == 40) { // DOWN
			highlighted_line++;
		} else if (processing.keyCode == 37) { // LEFT
			if(highlighted_index > 0) {
				highlighted_index--;
			}
		} else if (processing.keyCode == 39) { // RIGHT
			highlighted_index++;
		}
  	}
  }

  processing.keyReleased = function() {
  	if(processing.keyCode == 16) {
  		shift_down = false;
  	}
  }

  processing.setWords = function(listOfWords) {
  	words = listOfWords;
  }

  processing.addWords = function(listOfWords) {
    words = words.concat(listOfWords);
  }

  processing.setWordPrefix = function(prefix) {
    word_prefix = prefix;
  }
}

var canvas = document.getElementById("canvas1");
// attaching the sketchProc function to the canvas
var processingInstance = new Processing(canvas, sketchProc);
//processingInstance.setWords(["Saab","Volvo","BMW","waseem","ahmad","dennis","qian","ivan","hernandez","sal","testa","rick","song","ashrith","pillarisetti","Saab","Volvo","BMW","waseem","ahmad","dennis","qian","ivan","hernandez","sal","testa","rick","song","ashrith","pillarisetti"]);

function getRhymingWordList(word) {
  //TODO: clear div

  $.ajax({
  		url: "/rhyme?word="+word,
  		dataType: 'json',
  		success: function(data) { 
        //var words = new Array();
        //for(i=0;i<data.length;i++) {
        //  words.push(data[i]["word"]);
        //}

    $("#tags").html("");
        for(i=0;i<data.length;i++) {
          $("#tags").html($("#tags").html()+" <span>" + data[i]["word"] + "</span>");
        }
        //TODO: add spans to div

        //processingInstance.setWords(words);
        //alert(data[0]["word"]);  //TODO: shove into set words
      },
      error: function(data) { alert("error"); }
	});
}

function getRhymingWordListMany(wordlist) {
  //processingInstance.setWords(new Array());
  
  $("#tags").html("");
  for(j=0;j<wordlist.length;j++) {
    $.ajax({
      url: "/rhyme?word="+wordlist[j],
      dataType: 'json',
      invokeddata: wordlist[j],
      success: function(data) { 
        //var words = new Array();
        //for(i=0;i<data.length;i++) {
        //  words.push(data[i]["word"]);
        //}
        $("#tags").html($("#tags").html()+"<div id="+this.invokeddata+" class='results_block'><p id='word_title'>" + this.invokeddata + "</p></div>");
        
        var limit = 20;
        if(limit > data.length) {
          limit = data.length;
        }
        for(i=0;i<limit;i++) {
          $("#"+this.invokeddata).html($("#"+this.invokeddata).html()+" <span>" + data[i]["word"] + "</span>");
        }

        //processingInstance.addWords(words);
        //alert(data[0]["word"]);  //TODO: shove into set words
      },
      error: function(data) { alert("error"); }
    });
  }
}

var stopwords = ['a', 'about', 'above', 'after', 'again', 'against', 'all', 'am', 'an', 'and', 'any', 'are', "aren't", 'as', 'at', 'be', 'because', 'been', 'before', 'being', 'below', 'between', 'both', 'but', 'by', "can't", 'cannot', 'could', "couldn't", 'did', "didn't", 'do', 'does', "doesn't", 'doing', "don't", 'down', 'during', 'each', 'few', 'for', 'from', 'further', 'had', "hadn't", 'has', "hasn't", 'have', "haven't", 'having', 'he', "he'd", "he'll", "he's", 'her', 'here', "here's", 'hers', 'herself', 'him', 'himself', 'his', 'how', "how's", 'i', "i'd", "i'll", "i'm", "i've", 'if', 'in', 'into', 'is', "isn't", 'it', "it's", 'its', 'itself', "let's", 'me', 'more', 'most', "mustn't", 'my', 'myself', 'no', 'nor', 'not', 'of', 'off', 'on', 'once', 'only', 'or', 'other', 'ought', 'our', 'ours ', 'ourselves', 'out', 'over', 'own', 'same', "shan't", 'she', "she'd", "she'll", "she's", 'should', "shouldn't", 'so', 'some', 'such', 'than', 'that', "that's", 'the', 'their', 'theirs', 'them', 'themselves', 'then', 'there', "there's", 'these', 'they', "they'd", "they'll", "they're", "they've", 'this', 'those', 'through', 'to', 'too', 'under', 'until', 'up', 'very', 'was', "wasn't", 'we', "we'd", "we'll", "we're", "we've", 'were', "weren't", 'what', "what's", 'when', "when's", 'where', "where's", 'which', 'while', 'who', "who's", 'whom', 'why', "why's", 'with', "won't", 'would', "wouldn't", 'you', "you'd", "you'll", "you're", "you've", 'your', 'yours', 'yourself', 'yourselves'];
$("#text_input").keyup(function (event) { 
  var lines = $(this).val().split('\n');

  //filter lines to ignore blank lines (except for the newest line that's blank)
  var valid_lines = new Array();
  for(i=0;i<lines.length-1;i++) {
    if($.trim(lines[i]).length > 0) {
      valid_lines.push($.trim(lines[i]));
    }
  }
  valid_lines.push(lines[lines.length-1]);
  lines = valid_lines;

  if ( event.which == 13 ) { //enter key is pressed
    if(lines.length > 1) { //parse the line you just finished with
      var last_line = lines[lines.length-2];
      var last_line_words = last_line.split(' ');

      var usefulwords = [];
      for (i = 0; i < last_line_words.length; i++) {
        var isStop = $.inArray(last_line_words[i].toLowerCase(), stopwords);
        if (isStop == -1) {
          usefulwords.push(last_line_words[i]);
        }
      }
      getRhymingWordListMany(usefulwords);
    }
  } else { //any other key is pressed
    /*
    var current_line = lines[lines.length-1];
    var current_line_words = current_line.split(' ');
    var current_word = "";
    if(current_line_words[current_line_words.length-1] != "") {
      current_word = current_line_words[current_line_words.length-1];
    }
    */
  }
});

    /*
    //get the last word of the line above you, and rhyme it
    if(lines.length > 0) {
      var current_line = lines[lines.length-1];
      var above_line = lines[lines.length-2];

      var current_line_words = current_line.split(' ');
      var current_word = "";
      if(current_line_words[current_line_words.length-1] != "") {
        current_word = current_line_words[current_line_words.length-1];
      }
      processingInstance.setWordPrefix(current_word);
      var above_line_words = ($.trim(above_line)).split(' '); //TODO: add for above 3 lines
      
      //for 1 word
      //var rhyming_word = above_line_words[above_line_words.length-1];
      //getRhymingWordList(rhyming_word);
      
      //for all words
      var usefulwords = [];
      for (i = 0; i < above_line_words.length; i++) {
        var isStop = $.inArray(above_line_words[i].toLowerCase(), stopwords);
        if (isStop == -1) {
          usefulwords.push(above_line_words[i]);
        }
      }
      getRhymingWordListMany(usefulwords);
    }
    */
  //var last = lines.pop();
  //var secondlastline = lines.pop();
  //var secondlast = secondlastline.split(' ');
  ///*var isStop = jQuery.inArray(secondlast, stopwords);*/
  //var usefulwords = [];
  //for (i = 0; i < secondlast.length; i++) {
  //  var isStop = jQuery.inArray(secondlast[i].toLowerCase(), stopwords);
  //  if (isStop == -1) {
  //    usefulwords.push(secondlast[i]);
  //  }
  //}
  ///*var lastword = stopwords.pop();*/
  //$("custom").text(usefulwords);



