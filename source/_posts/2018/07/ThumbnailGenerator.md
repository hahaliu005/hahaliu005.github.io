---
title: Thumbnail Generator
tags: 
  - PHP
date: 2018-07-05
---

Use this function can generator a new image with any size , though it's sounds like this , don't use this function to generator the image that width and height big than original image , it will anamorphose badly .

<!-- more -->

```php
/**
 * the main use of this function is to generator thumbnail
 * make sure your GD library are enabled
 *
 * @param string $image_path           the image path
 * @param string $thumb_image_path     the generated thumbnail path
 * @param number $resize_width         thumbnail width
 * @param number $resize_heigth        thumbnail height
 * @return success return true , failue on false
 */
function image_resize($image_path,$thumb_image_path,$resize_width,$resize_heigth){
	$image_array = array();
	preg_match('/.+\.(\w+)/',$image_path,$match);
	switch ($match[1]){
		case 'gd2':
			$image = imagecreatefromgd2($image_path);
			break;
		case 'gd':
			$image = imagecreatefromgd($image_path);
			break;
		case 'gif':
			$image = imagecreatefromgif($image_path);
			break;
		case 'jpg':
			$image = imagecreatefromjpeg($image_path);
			break;
		case 'png':
			$image = imagecreatefrompng($image_path);
			break;
		case 'wbmp':
			$image = imagecreatefromwbmp($image_path);
			break;
		case 'xbm':
			$image = imagecreatefromxbm($image_path);
			break;
		case 'xpm':
			$image = imagecreatefromxpm($image_path);
			break;
		default:
			return false;
	}
	list($width,$height) = getimagesize($image_path);
	$newimage = imagecreatetruecolor($resize_width, $resize_heigth);
	imagecopyresampled($newimage, $image, 0, 0, 0, 0, $resize_width, $resize_heigth, $width, $height);
	if (imagejpeg($newimage,$thumb_image_path)) {
		imagedestroy($image);
		imagedestroy($newimage);
		return true;
	}else {
		imagedestroy($image);
		imagedestroy($newimage);
		return false;
	}
}
```