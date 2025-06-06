# MT1-1　課題１


 あんこを食べてポイントを稼ぐゲームなので、性質の異なる4種類のあんこを作りました。

 赤あんこはランダムな方向に飛び、復活までの時間は比較的短いです。速度のランダム補正があるため、プレイヤーが壁にぶつかる前に食べれば、さらに速度が上がる可能性があります。

 青あんこは小さくて捕まえにくいです。青あんこは赤あんこの逆で、壁にぶつかるとスピードが上がります。やがて追いつけないほど速くなってしまいます。

 みどりのあんこは動かないので、集めやすいです。ポイントは一番少ないですが、すぐに復活します。素早く集め続けることができれば、ポイントを早く貯めることができます。

 黒あんこは小さくて動きが遅いので、捕まえるのが最も難しいです。また、復活タイマーも長いので、消える前に捕まえないとポイントを逃してしまいます。


 

// 8方向の動きにはベクトルの手順をしようした。
float dirX = 0.0f;
float dirY = 0.0f;
float length = sqrtf((dirX * dirX) + (dirY * dirY));

float newX = dirX;
float newY = dirY;

if (length != 0.0f) {
	newX = dirX / length;
	newY = dirY / length;
	playerPosX += newX * playerSpeed;   <---対角速度は水平速度および垂直速度と同じになります。
	playerPosY += newY * playerSpeed; 

 //DASH
if ((keys[DIK_SPACE]) && (!preKeys[DIK_SPACE]) && (isPlayerDash == false)) {
	isPlayerDash = true;
}
if ((isPlayerDash == true) && (dashTimer >= 1)) {
	dashTimer--;
	playerSpeed = 20;    <---　Dash の速度 は普通の２倍です
}
if (dashTimer <= 0) {
	isPlayerDash = false;
	playerSpeed = 10;
	dashTimer = 20;      <---　Dash の時間
}


//回転してから移動
float M_PI = 3.14f;
float theta = 1.0f / (8.0f * M_PI);

const float kLeftTopX = -playerWidth / 2.0f;　　<----- Quad の角の位置
const float kLeftTopY = -playerHeight / 2.0f;
....
//回転
float leftTopRotatedX = (kLeftTopX * cosf(theta)) - (kLeftTopY * sinf(theta));
float leftTopRotatedY = (kLeftTopY * cosf(theta)) + (kLeftTopX * sinf(theta));
....
//移動
float leftTopPositionX = leftTopRotatedX + playerPosX;
float leftTopPositionY = leftTopRotatedY + playerPosY;
....
// DRAW QUAD
Novice::DrawQuad(static_cast<int>(leftTopPositionX), static_cast<int>(leftTopPositionY),　　<--- 回転して移動の後で　Quad の角の位置
	static_cast<int>(rightTopPositionX), static_cast<int>(rightTopPositionY),
	static_cast<int>(leftBottomPositionX), static_cast<int>(leftBottomPositionY),
	static_cast<int>(rightBottomPositionX), static_cast<int>(rightBottomPositionY),
	0, 0, 1, 1, kFillModeSolid, WHITE);


// あんこ を食べたら。。。
float distanceX1 = (playerPosX - anko1PosX) * (playerPosX - anko1PosX);
float distanceY1 = (playerPosY - anko1PosY) * (playerPosY - anko1PosY);
float distanceXY1 = sqrtf(distanceX1 + distanceY1);

if ((distanceXY1 <= anko1Radius + playerRadius) && isAnko1Alive == true) {   <--- distanceXY = プレイヤーとあんこの距離

if (isAnko1Alive == false) {
	respawnTimer1--;
	if (respawnTimer1 <= 0) {
		anko1Radius += (rand() % 41) - (rand() % 41);
		anko1PosX += (rand() % 641) - (rand() % 641);   <--- あんこはrespawnしたら位置(posX + posY)が変わる
		anko1PosY += (rand() % 361) - (rand() % 361);   
		anko1SpeedX += rand() % 17 - rand() % 7;        <--- あんこの速度も変わる
		anko1SpeedY += rand() % 17 - rand() % 7;
		isAnko1Alive = true;
		respawnTimer1 = 60;                <--- あんこはrespawnして二つのタイマーがリセットする
		disappearTimer1 = 100;
	}
 
//壁にぶつかった時に速度が変化するメカニックを実装した
float speedBumpX = -0.6f;      <---　速度の変化
float speedBumpY = -0.5f;
if (anko1PosX >= 1280 - anko1Radius) {
	anko1PosX = 1280 - anko1Radius;
	anko1SpeedX = anko1SpeedX * speedBumpX;  <---　あんこは壁にぶつかって速度が変化する
}
if (anko1PosX <= 0 + anko1Radius) {
	anko1PosX = anko1Radius;
	anko1SpeedX = anko1SpeedX * speedBumpX;


 
