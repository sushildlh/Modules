# SaveWorkForFuture


# Image Captured....

`private int REQUEST_IMAGE_CAPTURE = 100;`

### on Click ....   

       Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
       if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
           _startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
        }

### on ActivityResult ....

        if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
            // Bundle extras = data.getExtras();
            Bitmap bitmap =  ImageUtils.getBitmapFromIntent(this, data);
            mImage.setImageBitmap(bitmap);// mImage is a ImageView which is bind previously.
            String imgPath = ImageUtils.createFile(this, bitmap);
            File imageFile = new File(imgPath);            
        }

### and other Method....

     public class ImageUtils {
    
       public static Bitmap getBitmapFromIntent(Context context,Intent data) {
        Bitmap bitmap = null;
        
        if (data.getData() == null) {
            bitmap = (Bitmap) data.getExtras().get("data");
        } else {
            try {
                bitmap = MediaStore.Images.Media.getBitmap(context.getContentResolver(), data.getData());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        
        return bitmap;
    }
    
    public static void createCapturedImage(Context context, Intent data) {
        Bitmap bitmap = null;
        
        if (data.getData() == null) {
            bitmap = (Bitmap) data.getExtras().get("data");
        } else {
            try {
                bitmap = MediaStore.Images.Media.getBitmap(context.getContentResolver(), data.getData());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        
        createFile(context,bitmap);
    }
    
    public static String createFile(Context context, Bitmap data) {
        Uri selectedImage = getImageUri(context,data);
        String[] filePath = {MediaStore.Images.Media.DATA};
        Cursor c = context.getContentResolver().query(selectedImage, filePath, null, null, null);
        c.moveToFirst();
        c.getColumnIndex(filePath[0]);
        int columnIndex = c.getColumnIndex(filePath[0]);
        String picturePath = c.getString(columnIndex);
        c.close();
        
        return picturePath;
    }
    
    public static Uri getImageUri(Context context, Bitmap inImage) {
        ByteArrayOutputStream bytes = new ByteArrayOutputStream();
        inImage.compress(Bitmap.CompressFormat.JPEG, 100, bytes);
        String path = MediaStore.Images.Media.insertImage(context.getContentResolver(), inImage, "Pet_Image", null);
        return Uri.parse(path);
    }
    
    }
