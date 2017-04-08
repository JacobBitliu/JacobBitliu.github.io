---
layout:     default
title:      Use dom4j and Xpath to analyze xml file
category: blog
description: Use dom4j and Xpath to analyze xml file
---

#Prepare the jar file

    In order to use dom4j and Xpath the first thing is download the jar file you need .
    
    Dom4j code need two jar file . 
1. [dom4j.jar](https://dom4j.github.io)
2. [jaxen.jar](https://sourceforge.net/projects/jaxen/)

    Click and download the jar file , add it to your project .
    Next is one of my project , you can see my jar file.
    
![]({{site.url}}/assets/jarFileDom4j.png)

#Dom4j docs
    You can download the docs of dom4j from the link gave on the last step. In this docs , you can learn how to prase XML , read , write etc... In this step , I will give you a quice start of dom4j which most from the dom4j doc.

* parse XML 

Next is some code and implement from dom4j doc.
    
    One of the first things you'll probably want to do is to parse an XML document of some kind. This is easy to do in dom4j. The following code demonstrates how to this.
   
    code:
```
import java.net.URL;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.io.SAXReader;

public class Foo {

    public Document parse(URL url) throws DocumentException {
        SAXReader reader = new SAXReader();
        Document document = reader.read(url);
        return document;
    }
}
```

    From the code , my can know that dom4j read xml by using SAXReader() , except this , we also know it can read url .

* Navigate the xml file 


>    A document can be navigated using a variety of methods that return standard Java Iterators. For example:
    
```
public void bar(Document document) throws DocumentException {

        Element root = document.getRootElement();

        // iterate through child elements of root
        for ( Iterator i = root.elementIterator(); i.hasNext(); ) {
            Element element = (Element) i.next();
            // do something
        }

        // iterate through child elements of root with element name "foo"
        for ( Iterator i = root.elementIterator( "foo" ); i.hasNext(); ) {
            Element foo = (Element) i.next();
            // do something
        }

        // iterate through attributes of root 
        for ( Iterator i = root.attributeIterator(); i.hasNext(); ) {
            Attribute attribute = (Attribute) i.next();
            // do something
        }
     }
```

>    This method based on Iterator.In dom4j XPath expressions can be evaluated on the Document or on any Node in the tree (such as Attribute, Element or ProcessingInstruction). This allows complex navigation throughout the document with a single line of code. For example.
    
```
public void bar(Document document) {
        List list = document.selectNodes( "//foo/bar" );

        Node node = document.selectSingleNode( "//foo/bar/author" );

        String name = node.valueOf( "@name" );
    }

```

>   For example if you wish to find all the hypertext links in an XHTML document the following code would do the trick.
    
```
public void findLinks(Document document) throws DocumentException {

        List list = document.selectNodes( "//a/@href" );

        for (Iterator iter = list.iterator(); iter.hasNext(); ) {
            Attribute attribute = (Attribute) iter.next();
            String url = attribute.getValue();
        }
    }
```

>   If you need any help learning the XPath language we highly recommend the Zvon tutorial which allows you to learn by example.
    
  [Zvon](http://www.zvon.org/xxl/XPathTutorial/General/examples.html)

* Walk the xml

>    If you ever have to walk a large XML document tree then for performance we recommend you use the fast looping method which avoids the cost of creating an Iterator object for each loop. For example
    
```
public void treeWalk(Document document) {
        treeWalk( document.getRootElement() );
    }

    public void treeWalk(Element element) {
        for ( int i = 0, size = element.nodeCount(); i < size; i++ ) {
            Node node = element.node(i);
            if ( node instanceof Element ) {
                treeWalk( (Element) node );
            }
            else {
                // do something....
            }
        }
    }
```

    But I don't recommend you use this code to walk the xml tree , I'd like to use the code next .
    
```
    public static Element getRootElement(Document doc){//get root of the xml tree.
        return doc.getRootElement();
    }

    public static void treeWalk(Element element){//start recursion.
        for (Iterator i = element.elementIterator(); i.hasNext(); ){
            Element p = (Element) i.next();

            if (p.hasMixedContent()){
                Attribute attribute = p.attribute("name");//take the "name" Attribute.
                if (attribute!=null) {
                    System.out.println(attribute.getName() + ":" + attribute.getValue());
                }
                System.out.println(p.getName()+":path is "+p.getPath());
                treeWalk(p);//start the recursion.
            }else{
                System.out.println("test.xml last node: "+p.getPath()+" "+p.getText());

            }
        }
    }
```

* Creat a xml

>Often in dom4j you will need to create a new document from scratch. Here's an example of doing that.

```
import org.dom4j.Document;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;

public class Foo {

    public Document createDocument() {
        Document document = DocumentHelper.createDocument();
        Element root = document.addElement( "root" );

        Element author1 = root.addElement( "author" )
            .addAttribute( "name", "James" )
            .addAttribute( "location", "UK" )
            .addText( "James Strachan" );
        
        Element author2 = root.addElement( "author" )
            .addAttribute( "name", "Bob" )
            .addAttribute( "location", "US" )
            .addText( "Bob McWhirter" );

        return document;
    }
}
```

# My own code .

Next is my code in my project . 

```
public static void findInfobox(Document document) {

        Element rootElement = document.getRootElement();

        String outpath = "/Volumes/Slow Disk/GraduatonData/GraduationDataCSV/test.csv";

        CsvWriter writer = new CsvWriter(outpath, ',', Charset.forName("Utf8"));

        for (Iterator i = rootElement.elementIterator();i.hasNext();){

            Element p = (Element) i.next();

            String mainNodeName = p.valueOf("./@name");

            List<Node> inbox = p.selectNodes("./infobox/item");//this is right code.

            for (Iterator item = inbox.iterator();item.hasNext();){

                Element itemElement =(Element) item.next();

                //String rdf = mainNodeName;

                //writer.write(mainNodeName);

                //List<String> rdftemp;

                String[] rdftemp = new String[3];

                rdftemp[0] = mainNodeName;
                for (Iterator i1 = itemElement.elementIterator();i1.hasNext();){

                    Element p1 = (Element) i1.next();

                    if (p1.getName().equals("name")){
                        rdftemp[2] = p1.getText();
                        //rdf+=p1.getText()+",";

                        //writer.write(p1.getText());
                    }

                    if (p1.getName().equals("value")){
                        rdftemp[1] = p1.getText();
                        //rdf+=p1.getText()+")";

                        //writer.write(p1.getText());
                    }

                }

                //rdf p1 = new rdf(rdf);



                try {
                    rdfList.add(new rdf(rdftemp));
                    writer.writeRecord(rdftemp);
                }
                catch (Exception e){
                    System.out.println(e.getMessage());
                }

                //writer.writeRecord(rdf);
                //System.out.println(rdf);
            }
        }
    }
```
    
    Just as you see , I used Xpath to select the node I want , which is convenient .
    
    Next figure show my xml file tree which I use my code on it. 
    
![]({{site.url}}/assets/graduationXML.png)





