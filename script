const puppeteer = require("puppeteer");
const chalk = require("chalk");
var fs = require("fs");

// 
const error = chalk.bold.red;
const success = chalk.keyword("green");

(async () => {
  try {
    // open the headless browser
    var browser = await puppeteer.launch({ headless: false });
    // open a new page
    var page = await browser.newPage();
    // enter url in page
    await page.goto(`https://www.indeed.com/jobs?q=full+time&l=United+States`);
    await page.waitForSelector("div.result");



    var news = await page.evaluate(() => {
    	var dateList = document.querySelectorAll(`span.date`);
      var titleNodeList = document.querySelectorAll(`h2.jobtitle`);
      var locList = document.querySelectorAll(`span.location`);
      var compnyList = document.querySelectorAll(`span.company`);
      var salryList = document.querySelectorAll('span.salary.no-wrap');
      var synopList = document.querySelectorAll('div.paddedSummary');
      var titleLinkArray = [];
      for (var i = 0; i < titleNodeList.length; i++) {
        titleLinkArray[i] = {
        	date: dateList[i].innerText.trim(),
          jobtitle:titleNodeList[i].innerText.trim(),
          location:locList[i].innerText.trim(),
          company: compnyList[i].innerText.trim(),
          salary:salryList[i].innerText.trim(),
          synopsis: synopList[i].innerText.trim()

        };
      }
      return titleLinkArray;
      });

// console.log(news);
    await browser.close();

    
    // Writing the news inside a json file
    fs.writeFile("indeedjobs.json", JSON.stringify(news), function(err) {
      if (err) throw err;
      console.log("Saved!");
    });
    console.log(success("Browser Closed"));
  } catch (err) {
    // Catch and display errors
    console.log(error(err));
    await browser.close();
    console.log(error("Browser Closed"));
  }})();

