---
title: 留言板
date: 2020-10-24 17:00:00
top_img:
---

<style>
  #page-header {
    background: transparent !important;
  }
</style>



<head>
  <style media="screen">
  @media screen and (max-width: 530px) {
    #wrap {
          display:none!important;
      }
  }
  @media screen and (min-width: 530px) {
    #mobile {
          display:none!important;
      }
  }
    body {
      background: #F5F5F5;
      color: gray;
      font-family: tahoma;
    }

    p {
      font-family: tahoma;
      color: #757575;
    }
    
    #wrap {
      width: 530px;
      margin: 20px auto 0;
      height: 1000px;
    }
    
    h1 {
      margin-bottom: 20px;
      text-align: center;
      font-size: 35px;
      font-family: tahoma;
      color: black;
    }
    
    #form-wrap {
      overflow: hidden;
      height: 447px;
      position: relative;
      top: 0px;
      transition: all 1s ease-in-out .3s;
    }
    
    #form-wrap:before {
      content: "";
      position: absolute;
      bottom: 128px;
      left: 0px;
      background: url('https://cdn.jsdelivr.net/gh/Akilarlxh/Valine-Admin@v1.0/source/img/before.png');
      background-repeat: no-repeat;
      width: 530px;
      height: 317px;
    }
    
    #form-wrap:after {
      content: "";
      position: absolute;
      bottom: 0px;
      left: 0;
      background: url('https://cdn.jsdelivr.net/gh/Akilarlxh/Valine-Admin@v1.0/source/img/after.png');
      background-repeat: no-repeat;
      width: 530px;
      height: 259px;
    }
    
    #form-wrap.hide:after,
    #form-wrap.hide:before {
      display: none;
    }
    
    #form-wrap:hover {
      height: 1300px;
      top: -200px;
    }
    
    form {
      position: relative;
      top: 200px;
      overflow: visible;
      height: 800px;
      width: 500px;
      margin: 0px auto;
      padding: 5px;
      transition: all 1s ease-in-out .3s;
    }
    
    #form-wrap:hover form {
      height: 530px;
      display: fit-content;
    }
    
    #letter {
      background: white;
      width: 95%;
      max-width: 800px;
      margin: auto auto;
      border-radius: 5px;
      border: 1px solid;
      overflow: hidden;
      -webkit-box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.12);
      box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.18);
    }



    #title3 {
      text-decoration: none;
      color: rgb(246, 214, 175);
      float: left;
    }
    
    #comment {
      border-bottom: #ddd 1px solid;
      border-left: #ddd 1px solid;
      padding-bottom: 20px;
      background-color: #eee;
      margin: 15px 0px;
      padding-left: 20px;
      padding-right: 20px;
      border-top: #ddd 1px solid;
      border-right: #ddd 1px solid;
      padding-top: 20px;
      font-family: "Arial", "Microsoft YaHei", "黑体", "宋体", sans-serif;
    }
    
    #sitelink {
      text-transform: uppercase;
      text-decoration: none;
      font-size: 14px;
      border: 2px solid #6c7575;
      color: #2f3333;
      padding: 10px;
      display: inline-block;
      margin: 10px auto 0;
      background-color: rgb(246, 214, 175);
    }
  </style>
</head>

<body>
  <div id="wrap">
    <div id="form-wrap">
      <form>
        <div id="content">
          <div id="letter">
            <header style="overflow: hidden;"><img style="width:100%;z-index: 666;" src="https://ae01.alicdn.com/kf/U5bb04af32be544c4b41206d9a42fcacfd.jpg" /></header>
            <div style="padding: 5px 20px;"><br>
              <center>
                <h3 id="title3">来自Master的留言:</h3>
              </center><br><br>
              <center id="comment"><p style="font-size:1.2rem;font-weight:bold">
                <br>
                有什么想问的？<br>
                有什么想说的？<br>
                有什么想吐槽的？<br>
                哪怕是有什么想吃的，都可以告诉我哦~<br>
              </p></center><br>
              <div style="text-align: center;margin-top: 40px;"><img src="https://ae01.alicdn.com/kf/U0968ee80fd5c4f05a02bdda9709b041eE.png"  style="width:100%; margin:5px auto 5px auto; display: block;" /></div>
              <p style="font-size: 12px;text-align: center;color: #999;">自动书记人偶竭诚为您服务！<br></p>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
  <div id="mobile">
  <center id="comment"><p style="font-size:1.2rem;font-weight:bold">
    <br>
    有什么想问的？<br>
    有什么想说的？<br>
    有什么想吐槽的？<br>
    哪怕是有什么想吃的，都可以告诉我哦~<br>
  </p></center>
  </div>
</body>
