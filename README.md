<header class="navbar navbar-default">
  <div class="container">
    <div class="pageTitle">
      WS Software Distribution and End User Communication
    </div>
    <div class="user-info">
      <img *ngIf="photoUrl" src="{{photoUrl}}" style="">
      <img *ngIf="!photoUrl" src="http://fwdirectory.ms.com/itsmg/slash/webapp/images/IconNoPhotoAvailable_Large.png">
      <span class="user-name">{{userName}}</span>
    </div>
  </div>
</header>

.container {
  width: 100%!important;
  padding: 0;
  max-width: 100%!important;
}

.container .pageTitle {
  padding: 14px 20px;
  display: inline-block;
  font-family: Karla-Bold;
  font-size: 20px;
  color: #fff;
  letter-spacing: .25px;
  cursor: pointer;
}

.container .user-info {
  float: right;
  padding: 7px 20px 0;
  display: inline-block;
}

.container .user-info img {
  width: 20px;
  height: 20px;
  border-radius: 100%;
  margin-right: 8px;
  position: relative;
  top: 6px;
}

.container .user-info .user-name {
  font-family: Karla-Bold;
  color: #fff;
  letter-spacing: .25px;
  font-size: 15px;
}
