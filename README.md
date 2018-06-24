# Image Gallery

Image Gallery is plugin to display images based on prettyPhoto javascript library.

# How to Use

  Create a Region, choose Type "Image Gallery[Plug-In]" and in Region source enter following code:
 
     select 'f?p=&APP_ID.:0:&APP_SESSION.:APPLICATION_PROCESS=GETFILE:::FILE_ID:'||id SHOW_IMAGE
             ,FILENAME
     from your_table
  
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
        sys.htp.p('Cache-Control: max-age=3600');  -- tell the browser to cache for one hour, adjust as necessary
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


# Preview

![alt text](https://github.com/nhasko/ImageGallery/blob/master/preview.gif)
