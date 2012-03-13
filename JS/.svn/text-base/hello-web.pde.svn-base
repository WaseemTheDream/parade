String words = "test";

void setup()
{
  size(500,500);
  background(125);
  fill(255);

  textFont(createFont("Georgia", 24));
  textAlign(LEFT);
}

void draw() {
  background(255);
  fill(0);
  text(words,0,50);
}

void keyPressed() {
  // Backspace
  if (keyCode == BACKSPACE) {
    words = words.substring(0,words.length()-1);
  } else {
    words += key.toString();
  }
}