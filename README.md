# Creative-Coding-11.22-Homework

· 灵感来源： 
蒙德里安作品中有标志性的长方形、正方形以及网格线，配色也十分经典，如红黄蓝灰黑白。

· 作品目标： 
作品提炼采用蒙德里安经典的正方形和网格线元素，并且在配色方案上选取蒙德里安画作中出现的红黄蓝灰黑五色。画布背景上闪烁着不断变换颜色的网格线，并且每点击一下画布，就会出现一个正方形，正方形之间以及与画布边缘之间会产生碰撞，营造出节奏变换和频率震动。

· 设计思路：
1. 定义向量、画布、配色
2. 更新正方形位置并绘制，检查碰撞并在碰撞之后交换速度
3. 在背景上绘制蒙德里安网格线

代码：

ArrayList<Square> squares;
color[] mondrianColors = {
  color(1, 74, 151),   // 蓝色
  color(212, 18, 26),   // 红色
  color(240, 206, 6),   // 黄色
  color(14, 39, 33),    // 深绿
  color(224, 229, 231)  // 白色
};

void setup() {
  size(600, 600);
  colorMode(RGB);
  squares = new ArrayList<Square>();
}

void draw() {
  background(255);

  // 绘制蒙德里安配色的线条
  drawMondrianLines();

  // 更新和显示每个正方形
  for (Square sq : squares) {
    sq.update();
    sq.display();
    // 检查与其他正方形的碰撞
    for (Square other : squares) {
      if (sq != other && sq.intersects(other)) {
        sq.handleCollision(other);
      }
    }
  }
}

void mousePressed() {
  float x = mouseX;
  float y = mouseY;
  float side = random(40, 90);
  color col = getRandomColor();
  Square newSquare = new Square(x, y, side, col);
  squares.add(newSquare);
}

color getRandomColor() {
  int choice = int(random(5));
  return mondrianColors[choice];
}

class Square {
  float x, y;     // 位置
  float side;      // 边长
  float speedX, speedY; // 速度
  color col;       // 颜色

  Square(float x, float y, float side, color col) {
    this.x = x;
    this.y = y;
    this.side = side;
    this.col = col;
    this.speedX = random(-2, 2);
    this.speedY = random(-2, 2);
  }

  void update() {
    // 更新位置
    x += speedX;
    y += speedY;

    // 碰撞检测
    if (x < 0 || x > width - side) {
      speedX *= -1;
    }
    if (y < 0 || y > height - side) {
      speedY *= -1;
    }
  }

  void display() {
    // 绘制正方形，添加 noStroke() 去除描边
    noStroke();
    fill(col);
    rect(x, y, side, side);
  }

  // 碰撞检测
  boolean intersects(Square other) {
    return x < other.x + other.side &&
           x + side > other.x &&
           y < other.y + other.side &&
           y + side > other.y;
  }

  // 处理碰撞
  void handleCollision(Square other) {
    // 交换速度
    float tempSpeedX = speedX;
    float tempSpeedY = speedY;
    speedX = other.speedX;
    speedY = other.speedY;
    other.speedX = tempSpeedX;
    other.speedY = tempSpeedY;
  }
}

// 绘制蒙德里安配色的线条
void drawMondrianLines() {
  strokeWeight(0.5);
  for (int i = 0; i < width; i += 40) {
    stroke(mondrianColors[int(random(5))]);
    line(i, 0, i, height);
  }
  for (int j = 0; j < height; j += 40) {
    stroke(mondrianColors[int(random(5))]);
    line(0, j, width, j);
  }
  noStroke();
}
