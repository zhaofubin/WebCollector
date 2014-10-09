WebCollector
============

WebCollector is an open source Java crawler which provides some simple interfaces for crawling the Web。You can setup a multi-threaded web crawler in 5 minutes!

###INSTALL
Add dependency to your pom.xml

    <dependency>
        <groupId>cn.edu.hfut.dmic.webcollector</groupId>
        <artifactId>WebCollector</artifactId>
        <version>1.31</version>
    </dependency>

###DEMO
This DEMO extracts all the questions asked  at [http://www.zhihu.com/](http://www.zhihu.com/) .
You need to create a crawler class that extends BreadthCrawler.


    public class ZhihuCrawler extends BreadthCrawler{
 
        /**
         * This function is called when a page is fetched and
         * ready to be processed by your program.       
        */
        @Override
        public void visit(Page page) {
            String question_regex="^http://www.zhihu.com/question/[0-9]+";         
            if(Pattern.matches(question_regex, page.getUrl())){              
                System.out.println("processing "+page.getUrl());

                /*extract title of the page*/
                String title=page.getDoc().title();
                System.out.println(title);

                /*extract the content of question*/
                String question=page.getDoc().select("div[id=zh-question-detail]").text();
                System.out.println(question);
             
            }
        }
 
        /**
         * start crawling
        */
        public static void main(String[] args) throws IOException{  
            ZhihuCrawler crawler=new ZhihuCrawler();
            crawler.addSeed("http://www.zhihu.com/question/21003086");
            crawler.addRegex("http://www.zhihu.com/.*");
            /*start the crawler with depth=5*/
            crawler.start(5);  
        }
 
   
    }

As can be seen in the above code,there are one function that should be overridden:
+ __visit(Page page):__ This function is called after the content of a URL is downloaded successfully.You can easily get the url,text of the downloaded page.If the Content-Type of the downloaded page is text/html,you could also get the document and html of the page.The document is a dom tree parsed by JSOUP.The html is a String decoded by detected charset.Page is an instance of cn.edu.hfut.dmic.webcollector.model.Page
 * page.getUrl() returns the url of the downloaded page
 * page.getContent() returns the origin data of the page
 * page.getDoc() returns an instance of org.jsoup.nodes.Document
 * page.getResponse() returns the http response of the page
 * page.getFetchTime() returns the time this page be fetched at  generated by System.currentTimeMillis()



###The architecture of WebCollector

![](https://github.com/CrawlScript/WebCollector/raw/master/webcollector_design.png)
 


__中文教程:__ [https://github.com/CrawlScript/WebCollector/blob/master/README.zh-cn.md](https://github.com/CrawlScript/WebCollector/blob/master/README.zh-cn.md)
