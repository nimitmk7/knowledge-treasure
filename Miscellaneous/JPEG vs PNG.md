JPEG is a lossy format which means it throws away some data to get a higher compression rate, while PNG does not.  
  
PNG tries to balance between the sizes of the image and the exactness (quality) of the image using a compression technique called DEFLATE which uses a combination of LZ77 and Huffman.  
  
LZ77 finds the repeating patterns in the image data, storing a reference to them instead of the entire data itself. For example, if the next 100 pixels are red in color, instead of storing color for them individually, it combines the information and stores it as "next 100 pixels are red"; thus saving space when colors are consecutively repeated.  
  
Huffman Coding helps in assigning shorter codes to more frequent colors and longer codes to less frequent colors, saving significant space.  
  
This indicates that PNG is a great format for images that contain fewer colors and patterns; but if the image is vibrant and contains lots of patterns the file size might be larger than the JPEG counterpart.

## References
1. https://www.linkedin.com/posts/arpitbhayani_this-afternoon-i-got-curious-about-image-activity-7171840360719998976-HHqz/

### Further reading
https://www.kaggle.com/code/kambojharyana/who-stole-my-pixels

