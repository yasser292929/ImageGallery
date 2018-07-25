# Image Gallery

Image Gallery is plugin to display images based on prettyPhoto javascript library.

# Changelog
1.1.2 fix for APEX 5.0 and APEX 5.1

1.1.1 css fix

1.1 Added options for link to other page

1.0 Initial Release
# How to Use

  Create a Region, choose Type "Image Gallery[Plug-In]" and in Region source enter following code:
 
     select 'f?p=&APP_ID.:0:&APP_SESSION.:APPLICATION_PROCESS=GETFILE:::FILE_ID:'||your_image_id SHOW_IMAGE, 
      your_image_name FILENAME,
      your_image_id IMAGE_ID,
      APEX_UTIL.PREPARE_URL(
        p_url => 'f?p=' ||:APP_ID || ':4:'||:APP_SESSION||'::NO::P4_ID:'||your_image_id,
        p_checksum_type => 'SESSION') IMAGE_URL
      from your_table
      
      
  Your Sql Query must have same alias as example. For IMAGE_URL you can add link to other page or set it to null.
  
  Create Application Process onDemand GETFILE with following code:
  
    begin
    for c1 in (select *
                 from your_table
                where id = :FILE_ID) loop
        --
        sys.htp.init;
        sys.owa_util.mime_header( c1.mime_type, FALSE );
        sys.htp.p('Content-length: ' || sys.dbms_lob.getlength( c1.content));
        sys.htp.p('Content-Disposition: attachment; filename="' || c1.filename || '"' );
        sys.htp.p('Cache-Control: no-cache');  
        sys.owa_util.http_header_close;
        sys.wpg_docload.download_file( c1.content );
     
        apex_application.stop_apex_engine;
    end loop;
    end;
    
  Create Application Item FILE_ID where we store ID of image
  
# Options 

1. Style

you can choose different style of prettyPhoto image Gallery


2. Autoplay slideshow

with or without auto slideshow

3. Animation speed

4. Slideshow

Duration of slideshow in miliseconds

5. Link icon

Icon for random link


# Preview

![alt text](https://github.com/nhasko/ImageGallery/blob/master/preview.gif)
