## What is XML ?
Extensible Markup Language (XML) lets you define and store data in a shareable manner. XML supports information exchange between computer systems such as websites, databases, and third-party applications. Predefined rules make it easy to transmit data as XML files over any network because the recipient can use those rules to read the data accurately and efficiently.

## Why is it important ?
Extensible Markup Language(XML) is a markup language that provides rules to define any data. It is a

Unlike other programming languages, XML cannot perform computing operations by itself. Instead, any programming language or software can be implemented for structured data management.

XML uses markup symbols to provide more information about any data. Other software, like browsers and data processing applications, use this information to process structured data more efficiently.

> [!faq] What is a Markup Language ? 
> 
>  A markup language is a set of rules that defines how the layout and presentation of text and images should appear in a digital document. It allows structuring documents, adding formatting, and specifying how different elements should be displayed (or “rendered”) on webpages.

### **XML tags**
You use markup symbols, called tags in XML, to define data. For example, to represent data for a bookstore, you can create tags such as `<book>`, `<title>`, and `<author>`. Your XML document for a single book would have content like this:
```xml
<book>

<title> Learning Amazon Web Services </title>

<author> Mark Wilkins </author>

</book>
```

Tags bring sophisticated data coding to integrate information flows across different systems.

## What are the applications of XML?
Extensible Markup Language (XML) is the underlying technology in thousands of applications, ranging from common productivity tools like word processing to book publishing software and even complex application configuration systems.

### Data Transfer
You can use XML to transfer data between two systems that store the same data in different formats. 

#### Example
Your website stores dates in MM/DD/YYYY format, but your accounting system stores dates in DD/MM/YYYY format. 

You can transfer the data from the website to the accounting system by using XML. Your developers can write code that automatically converts the following:

- Website data to XML format
- XML data to accounting system data
- Accounting system data back to XML format
- XML data back to website data

### Web Applications
XML gives structure to the data that you see on webpages. Other website technologies, like HTML, work with XML to present consistent and relevant data to website visitors. 
#### Example
Consider an e-commerce website that sells clothes. Instead of showing all clothes to all visitors, the website uses XML to create customized webpages based on user preferences. It shows products from specific brands by filtering the `<brand>` tag.

### Documentation
You can use XML to specify the structural information of any technical document. Other programs then process the document structure to present it flexibly. 

#### Example
There are XML tags for a paragraph, an item in a numbered list, and a heading. Using these tags, other types of software automatically prepare the document for uses such as printing and webpage publication

### Data Type
Many programming languages support XML as a data type. With this support, you can easily write programs in other languages that work directly with XML files.

## Components of XML file
An Extensible Markup Language (XML) file is a text-based document that ==you can save with the .xml extension==. You can write XML similar to other text files. To create or edit an XML file, you can use any of the following: 

- Text editors like Notepad or Notepad++
- Online XML editors
- Web browsers

### XML document
The `<xml></xml>` tags are used to mark the beginning and end of an XML file. The content within these tags is also called an XML document. It is the first tag that any software will look for to process XML code.

### **XML declaration**

An XML document begins with some information about XML itself. For example, it might mention the XML version that it follows. This opening is called an XML declaration. Here's an example.

`<?xml version="1.0" encoding="UTF-8"?>`

### **XML elements**
All the other tags you create within an XML document are called XML elements. XML elements can contain these features:
- Text
- Attributes
- Other elements

All XML documents begin with a primary tag, which is called the root element.
For example, consider the XML file below.
```XML
<InvitationList>

<family>

       <aunt>

       <name>Christine</name>

        <name>Stephanie</name>

       </aunt>

</family>

</InvitationList>
```

`<InvitationList>` is the root element; _family_ and _aunt_ are other element names.

### **XML attributes**

XML elements can have other descriptors called attributes. You can define your own attribute names and write the attribute values within quotation marks as shown below.

`<person age=“22”>`

Here person is the element, and age is the attribute.

### **XML content**

The data in XML files is also called XML content. For example, in the XML file, you might see data like this. 

```
<friend>

       <name>Charlie</name>

       <name>Steve</name>

</friend>
```
The data values _Charlie_ and _Steve_ are the content.

## What is an XML schema?

An Extensible Markup Language (XML) schema is a document that describes some rules or limits on the structure of an XML file. You can describe these constraints in several different ways, like these:
- Grammatical rules to determine the order of elements
- Yes or No conditions that the content must satisfy
- Data types for the content in XML files
- Constraints for data integrity

### Example
For example, an XML schema for bookstores might impose constraints like these:
1. A book element will have the attributes _title_ and _author_.
2. The book element will be nested under a category element with an attribute name.
3. The price of a book will be a separate element nested under _book_.

To meet these constraints, we will write the XML file as shown below. 

```XML
<category name=“Technology”>

<book title=“Learning Amazon Web Services”, author=“Mark Wilkins”>

<price>$20</price>

</book>

</category>
```

## What is an XML parser?
An extensible Markup Language(XML) parser is a software that can ==process or read XML documents to extract the data within them==. 

XML parsers also check the syntax or rules of the XML file, and can validate it against a particular XML schema. Because XML is a strict markup language, the parsers will not process the file if there are any validation or syntax errors. 

### Example
The XML parser will give errors if any of these conditions are true:

- A closing tag or end tag is missing
- Attribute values don’t have quotation marks
- A schema condition has not been met

### Use
Software applications use XML parsers to transform XML files into native data types. They can thus ==focus on the application logic without having to go into the details of the XML itself==.

## XML vs HTML

|              | XML                                        | HTML                     |
| ------------ | ------------------------------------------ | ------------------------ |
| Purpose      | Store and transport data                   | Present and display data |
| Tags         | Users can create and define their own tags | Predefined tags          |
| Syntax Rules | Case Sensitive                             | Not Case Sensitive       |

## References
1. [What is XML ? - Amazon Web Services](https://aws.amazon.com/what-is/xml/)
2. 
