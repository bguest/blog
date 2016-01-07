Title: NSAttributedString+UIImage  
Subtitle: A story of the missing NSTextAttachment link
Comments: True  
Date: January 7, 2016  

Adding images to an attributed string used to be a kind of tedious process that involved first
creating a `NSTextAttachment` Instead I just added these extensions to `NSAttributedString` and
`NSMutableAttributedString`

```swift

extension NSAttributedString{
    convenience init(image: UIImage, origin: CGPoint){
        let attachment = NSTextAttachment()
        attachment.image = image
        attachment.bounds = CGRect(origin: origin, size: image.size)
        self.init(attachment: attachment)
    }
}

extension NSMutableAttributedString{

    func appendImage(image: UIImage, origin: CGPoint){
        let attributed = NSAttributedString(image: image, origin: origin)
        self.appendAttributedString(attributed)
    }

    func replaceCharactersInRange(range: NSRange, withImage image:UIImage, origin:CGPoint){
        let attributed = NSAttributedString(image: image, origin: origin)
        self.replaceCharactersInRange(range, withAttributedString: attributed)
    }

    func replaceTextAttachmentWithImage(image: UIImage){
        enumerateAttribute(NSAttachmentAttributeName, inRange: NSMakeRange(0, self.length),
            options: NSAttributedStringEnumerationOptions(),
            usingBlock: {(value:AnyObject?, range:NSRange, stop:UnsafeMutablePointer<ObjCBool>) in
                if let attachment = value as? NSTextAttachment{
                    let origin = attachment.bounds.origin
                    self.replaceCharactersInRange(range, withImage: image, origin: origin)
                    stop[0] = true
                }
        })
    }
}
```
**Note** If you read the code you might realize that this only works for the first image in the
string, you could replace all the images in the string by removing the `stop[0] = true`. If I add
some more general manipulations I'll add them here.

Here's how you might use them:

```swift

let attributedText = NSMutableAttributedString(string: "FOO")
let image1 = UIImage(named: "image1")!
attributedText.appendImage(image1, origin: CGPoint(x: 5, y: -5))
attributedText.appendAttributedString(NSAttributedString(string: "BAR"))
attributedText

let image2 = UIImage(named: "image2")!
attributedText.replaceTextAttachmentWithImage(image2)
```
