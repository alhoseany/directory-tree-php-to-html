# Directory-tree-php-to-html
convert directory to html file by php

i try to find any way to convert directory to html array or json code and i cudn't find any way, so i try to create a php fuction that create a directory to an json code.

and i turn this code into a html.

# How to use 
* open file json.php in the directory you want to run.
<code>
 function dir_tree($dir, $as_json = false) {
            $files = [];
            foreach( glob($dir . "./*") as $file ) {
                if ( is_file($file) ) {
                    $files[$file] = $file;
                } else {
                    $files[$file] = dir_tree($file, false);
                }
            }
            return $as_json ? json_encode($files) : $files;
        }
       $file = dir_tree("./",true);
       $fp = fopen("Json-Text.txt","wb");
        if( $fp == false ){
            echo 'There seems to be something wrong';
        }else{
            fwrite($fp, $file);
            fclose($fp);
            echo 'File "Json-Text.txt" successfully created.';
        }
</code>

* you will have a text file caled 'Json-Text.txt'.
<pre>{".\/.\/New folder":[],".\/.\/file.php":".\/.\/file.php",".\/.\/geodata.php":".\/.\/geodata.php",".\/.\/index.html":".\/.\/index.html",".\/.\/index.php":".\/.\/index.php",".\/.\/mac.php":".\/.\/mac.php",".\/.\/old":{".\/.\/old.\/00.php":".\/.\/old.\/00.php",".\/.\/old.\/1.php":".\/.\/old.\/1.php",".\/.\/old.\/10.php":".\/.\/old.\/10.php",".\/.\/old.\/11.php":".\/.\/old.\/11.php",".\/.\/old.\/2.php":".\/.\/old.\/2.php",".\/.\/old.\/3.php":".\/.\/old.\/3.php",".\/.\/old.\/4.php":".\/.\/old.\/4.php",".\/.\/old.\/6.php":".\/.\/old.\/6.php",".\/.\/old.\/7.php":".\/.\/old.\/7.php",".\/.\/old.\/8.php":".\/.\/old.\/8.php",".\/.\/old.\/9.php":".\/.\/old.\/9.php"},".\/.\/test_app":{".\/.\/test_app.\/1.php":".\/.\/test_app.\/1.php",".\/.\/test_app.\/10.php":".\/.\/test_app.\/10.php",".\/.\/test_app.\/2.php":".\/.\/test_app.\/2.php",".\/.\/test_app.\/3.php":".\/.\/test_app.\/3.php",".\/.\/test_app.\/4.php":".\/.\/test_app.\/4.php",".\/.\/test_app.\/5.php":".\/.\/test_app.\/5.php",".\/.\/test_app.\/6.php":".\/.\/test_app.\/6.php",".\/.\/test_app.\/7.php":".\/.\/test_app.\/7.php",".\/.\/test_app.\/8.php":".\/.\/test_app.\/8.php",".\/.\/test_app.\/9.php":".\/.\/test_app.\/9.php",".\/.\/test_app.\/css":{".\/.\/test_app.\/css.\/style.css":".\/.\/test_app.\/css.\/style.css"},".\/.\/test_app.\/functions.php":".\/.\/test_app.\/functions.php",".\/.\/test_app.\/home.php":".\/.\/test_app.\/home.php",".\/.\/test_app.\/images":{".\/.\/test_app.\/images.\/flash-logo.png":".\/.\/test_app.\/images.\/flash-logo.png",".\/.\/test_app.\/images.\/quicktime-logo.gif":".\/.\/test_app.\/images.\/quicktime-logo.gif",".\/.\/test_app.\/images.\/t_1.jpg":".\/.\/test_app.\/images.\/t_1.jpg",".\/.\/test_app.\/images.\/t_2.jpg":".\/.\/test_app.\/images.\/t_2.jpg",".\/.\/test_app.\/images.\/t_3.jpg":".\/.\/test_app.\/images.\/t_3.jpg",".\/.\/test_app.\/images.\/t_4.jpg":".\/.\/test_app.\/images.\/t_4.jpg",".\/.\/test_app.\/images.\/t_5.jpg":".\/.\/test_app.\/images.\/t_5.jpg"},".\/.\/test_app.\/includes":{".\/.\/test_app.\/includes.\/footer.php":".\/.\/test_app.\/includes.\/footer.php",".\/.\/test_app.\/includes.\/header.php":".\/.\/test_app.\/includes.\/header.php",".\/.\/test_app.\/includes.\/navigation.txt":".\/.\/test_app.\/includes.\/navigation.txt"},".\/.\/test_app.\/index.php":".\/.\/test_app.\/index.php",".\/.\/test_app.\/js":{".\/.\/test_app.\/js.\/myscript.js":".\/.\/test_app.\/js.\/myscript.js",".\/.\/test_app.\/js.\/scripts.js":".\/.\/test_app.\/js.\/scripts.js"}},".\/.\/text.php":".\/.\/text.php",".\/.\/url":{".\/.\/url.\/src":{".\/.\/url.\/src.\/base_facebook.php":".\/.\/url.\/src.\/base_facebook.php",".\/.\/url.\/src.\/facebook.php":".\/.\/url.\/src.\/facebook.php",".\/.\/url.\/src.\/fb_ca_chain_bundle.crt":".\/.\/url.\/src.\/fb_ca_chain_bundle.crt",".\/.\/url.\/src.\/index.php":".\/.\/url.\/src.\/index.php"},".\/.\/url.\/url.php":".\/.\/url.\/url.php"},".\/.\/url.zip":".\/.\/url.zip"}</pre>
* edit index.php file and insert the 'Json-Text.txt' content into $json variable

![alt text](http://nasssar.me/wp-content/uploads/2017/05/newww.png)


# wordpress

and this code can make a shortcode

<pre>
function file_tree_func( $atts, $content = "" ) {
         $array = array (
        '<p>' => '', 
        '</p>' => '', 
        '<br />' => ''
        );
         $atts = shortcode_atts( array(
    		'json' => ''
    	), $atts, 'file_tree_func' );
        $content = strtr($content, $array);

        if(is_single()){
            
                function foreach_files($files){
                    $files =  str_replace("././","",$files);
                    $webwecho = '';
                     foreach ($files as $k => $file){   
                        $file_cleaned =  str_replace((basename($k)) . "./","",$file);             
                         if(is_array($file)){
                            $webwecho .= '<li class="is-folder"><span>';
                            $webwecho .= basename($k);
                            $webwecho .= '</span>';
                            $webwecho .='<ul>';
                            $webwecho .= foreach_files($file_cleaned);
                            $webwecho .='</ul></li>';
                         }else {
                             $webwecho .= '<li class="is-file"><span>'.basename($k).'</span>';
                             $webwecho .= '</li>';
                         }
                    }
                    return $webwecho;
                }

                 $jsoncode = json_decode($atts['json'], true);
  
                     $webwechoo = '';
                    $webwechoo .= '<div class="file-tree"> <ul>';
                    $webwechoo .= foreach_files($jsoncode);
                    $webwechoo .= '</ul> </div>';
                    return $webwechoo;

        }
        

  
            
}
add_shortcode( 'file-tree', 'file_tree_func' );

</pre>
