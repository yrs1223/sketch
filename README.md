// 宣告全域變數
let player1;
let player2;

function setup() {
  createCanvas(800, 600);
  
  // 初始化角色1
  player1 = {
    x: 200,
    y: 300,
    speedX: 5,
    speedY: 0,
    gravity: 0.8,
    jumpForce: -15,
    isJumping: false,
    groundY: 400,
    direction: 1,
    health: 100,
    color: color(255, 0, 0) // 紅色
  };
  
  // 初始化角色2
  player2 = {
    x: 600,
    y: 300,
    speedX: 5,
    speedY: 0,
    gravity: 0.8,
    jumpForce: -15,
    isJumping: false,
    groundY: 400,
    direction: -1,
    health: 100,
    color: color(0, 0, 255) // 藍色
  };
}

function draw() {
  background(220);
  
  // 更新物理
  updatePhysics(player1);
  updatePhysics(player2);
  
  // 檢查按鍵
  checkKeys();
  
  // 繪製角色
  drawPlayer(player1);
  drawPlayer(player2);
  
  // 繪製地板
  stroke(0);
  line(0, 400, width, 400);
  
  // 繪製生命值
  drawHealth();
}

function drawPlayer(player) {
  push();
  fill(player.color);
  noStroke();
  
  // 繪製身體
  rectMode(CENTER);
  rect(player.x, player.y, 40, 80);
  
  // 繪製頭部
  circle(player.x, player.y - 50, 30);
  
  // 繪製方向指示器（眼睛）
  fill(255);
  let eyeX = player.x + (player.direction * 8);
  circle(eyeX, player.y - 50, 10);
  
  pop();
}

function updatePhysics(player) {
  // 重力效果
  if (player.y < player.groundY) {
    player.speedY += player.gravity;
    player.y += player.speedY;
  }
  
  // 著地檢測
  if (player.y > player.groundY) {
    player.y = player.groundY;
    player.speedY = 0;
    player.isJumping = false;
  }
}

function checkKeys() {
  // 玩家1控制
  if (keyIsDown(65)) { // A
    player1.x -= player1.speedX;
    player1.direction = -1;
  }
  if (keyIsDown(68)) { // D
    player1.x += player1.speedX;
    player1.direction = 1;
  }
  
  // 玩家2控制
  if (keyIsDown(LEFT_ARROW)) {
    player2.x -= player2.speedX;
    player2.direction = -1;
  }
  if (keyIsDown(RIGHT_ARROW)) {
    player2.x += player2.speedX;
    player2.direction = 1;
  }
}

function keyPressed() {
  // 玩家1跳躍
  if ((key === 'w' || key === 'W') && !player1.isJumping) {
    player1.speedY = player1.jumpForce;
    player1.isJumping = true;
  }
  
  // 玩家2跳躍
  if (keyCode === UP_ARROW && !player2.isJumping) {
    player2.speedY = player2.jumpForce;
    player2.isJumping = true;
  }
}

function drawHealth() {
  // 玩家1血條
  fill(255, 0, 0);
  rect(50, 20, 200, 20);
  fill(0, 255, 0);
  rect(50, 20, player1.health * 2, 20);
  
  // 玩家2血條
  fill(255, 0, 0);
  rect(550, 20, 200, 20);
  fill(0, 255, 0);
  rect(550, 20, player2.health * 2, 20);
  
  // 顯示數值
  fill(0);
  textSize(16);
  text(`P1: ${player1.health}`, 50, 55);
  text(`P2: ${player2.health}`, 550, 55);
}

