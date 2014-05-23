<?php
class captcha{
	
	private $text = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
	private $char_count = 5;
	private $width = 100;
	private $height = 40;
	
	function __construct(){
		if(!session_id()){
			session_start();
		}
		$_SESSION['security_number'] = $this->generateText();
		$this->createCaptcha();
	}
	
	private function generateText(){
		$i = $this->char_count;
		$tot_text_len = strlen($this->text);
		$code = '';
		for($j=1;$j<=$i;$j++){
			$code .= substr($this->text,rand(1,$tot_text_len-1),1);
		}
		return $code;
	}
	
	private function createCaptcha(){
		$img = @imagecreatetruecolor($this->width, $this->height);
		$white = imagecolorallocate($img, 255, 255, 255); // for creating a image with white background
		imagefill($img, 0, 0, $white);

		$red = rand(100,255); 
		$green = rand(100,255);
		$blue = rand(100,255);
		
		$text_color = imagecolorallocate($img,255-$red,255-$green,255-$blue);
		$text = imagettftext($img,16,0,rand(10,25),rand(20,30),$text_color,"fonts/courbd.ttf",$_SESSION['security_number']);
		header("Content-type:image/jpeg");
		header("Content-Disposition:inline ; filename=secure.jpg");	
		imagejpeg($img);
	}

}

$cap = new captcha;
?>
