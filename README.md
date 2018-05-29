Google maps og brugen af dem. 


I mit program har jeg valgt at bruge google maps til at sætte nogle markøre som jeg selv har sat og som brugerne af programmet vil kunne se på kortet. Det er så meningen at markørerne skal være sat steder som skulle være af interesse for brugeren. 
Det er kan være ret så simpelt at sætte det op, især hvis programmet gør som det skal og ikke lige pludselig “glemmer” af autogenerere visse elementer. 

Så lad os starte fra bunden. Jeg antager at du kan finde ud af at sætte et android studio program op og kan finde ud af at tilføje nye aktiviteter. 

Så det første man gør, er at tilføje en ny aktivitet, den aktivitet man skal vælge, er den der hedder google maps aktivitet. Når aktiviteten er valgt vil den skabe en klasse ned i den folder der hedder Values (values er en undermappe af res). Klassen hedder google.maps.api.xml, og det er her at du skal ind og redigere lidt for at det kommer til at virke, for du skal nemlig selv sætte api’en, dette er dog ganske nemt og der er allerede en guide i selve den autogenereret klasse der fortæller dig hvordan du gør. Men for at gøre det så nemt som muligt vil du kunne se indholdet at klassen her:  <resources>
   <!--
   TODO: Before you run your application, you need a Google Maps API key.

   To get one, follow this link, follow the directions and press "Create" at the end:

   https://console.developers.google.com/flows/enableapi?apiid=maps_android_backend&keyType=CLIENT_SIDE_ANDROID&r=EF:B4:05:88:B9:BE:46:56:F8:05:D2:75:66:48:17:7E:E5:B5:6A:B0%3Bandroid.app.androidapp.maphandler

   You can also add your credentials to an existing key, using these values:

   Package name:
   EF:B4:05:88:B9:BE:46:56:F8:05:D2:75:66:48:17:7E:E5:B5:6A:B0

   SHA-1 certificate fingerprint:
   EF:B4:05:88:B9:BE:46:56:F8:05:D2:75:66:48:17:7E:E5:B5:6A:B0

   Alternatively, follow the directions here:
   https://developers.google.com/maps/documentation/android/start#get-key

   Once you have your key (it starts with "AIza"), replace the "google_maps_key"
   string in this file.
   -->
   <string name="google_maps_key" templateMergeStrategy="preserve" translatable="false">AIzaSyB9mxtnWt14HaGRoXoZ-25c8EmxC6O</string>
</resources>

Dog vil jeg foretrække at du selv går ind og finder en api og ikke bruger min derfor er det sidste 4 tegn heller ikke med i eksemplet.

Når det så er gjort, er det på tide at du laver nogle markøre på kortert. Dette er gange nemt og din aktivitet kommer med nogle præsatte markøre som henviser til Sidny. Dog hvis det ikke er det som dit kort skal vise markøren hen til, så vil jeg råde dig til at ændre dem. Jeg har personligt lavet et mad-kort der skal kunne hjælpe folk til at kunne finde mad.
For forklarings skyld vil jeg nu vise jer min klasse og så vil jeg forklare bid for bid hvad jeg har gjort og hvorfor. 
Min klasse MapActivityFood:    

package zombie.app.androidapp.maphandler;

import zombie.app.androidapp.R;
import android.support.v4.app.FragmentActivity;
import android.os.Bundle;
import android.widget.Toast;

import com.google.android.gms.maps.CameraUpdateFactory;
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.OnMapReadyCallback;
import com.google.android.gms.maps.SupportMapFragment;
import com.google.android.gms.maps.model.LatLng;
import com.google.android.gms.maps.model.Marker;
import com.google.android.gms.maps.model.MarkerOptions;

public class MapsActivityFood extends FragmentActivity implements OnMapReadyCallback, GoogleMap.OnMarkerClickListener {


   private static final LatLng Netto = new LatLng( -55.7601109, 12.456512900000007 );
   private static final LatLng Rema1000 = new LatLng(55.755948,12.475013999999987);

   private Marker mNetto;
   private Marker mRema1000;

   private GoogleMap foodmap;

   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(zombie.app.androidapp.R.layout.activity_maps_food);
       // Obtain the SupportMapFragment and get notified when the map is ready to be used.
       SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
               .findFragmentById(zombie.app.androidapp.R.id.map);
       mapFragment.getMapAsync(this);
   }


   /**
    * Manipulates the map once available.
    * This callback is triggered when the map is ready to be used.
    * This is where we can add markers or lines, add listeners or move the camera. In this case,
    * we just add a marker near Sydney, Australia.
    * If Google Play services is not installed on the device, the user will be prompted to install
    * it inside the SupportMapFragment. This method will only be triggered once the user has
    * installed Google Play services and returned to the app.
    */
   @Override
   public void onMapReady(GoogleMap map) {
     foodmap = map;



       // Add a marker
       mNetto = foodmap.addMarker(new MarkerOptions()
               .position(Netto)
               .title("netto"));
       mNetto.setTag(0);


       mRema1000 =foodmap.addMarker(new MarkerOptions()
       .position(Rema1000)
       .title("rema1000"));
       mRema1000.setTag(0);

      foodmap.setOnMarkerClickListener(this);

      }

   /** Called when the user clicks a marker. */
   @Override
   public boolean onMarkerClick(final Marker marker) {

       // Retrieve the data from the marker.
       Integer clickCount = (Integer) marker.getTag();

       // Check if a click count was set, then display the click count.
       if (clickCount != null) {
           clickCount = clickCount + 1;
           marker.setTag(clickCount);
           Toast.makeText(this,
                   marker.getTitle() +
                           " has been clicked " + clickCount + " times.",
                   Toast.LENGTH_SHORT).show();
       }

       // Return false to indicate that we have not consumed the event and that we wish
       // for the default behavior to occur (which is for the camera to move such that the
       // marker is centered and for the marker's info window to open, if it has one).
       return false;
   }
}


Som du kan se har jeg sat nogle markører på mit google maps, det har jeg gjort ved først at sætte    private static final LatLng Netto = new LatLng( -55.7601109, 12.456512900000007 ); 
Det jeg gør her er at jeg sætte et object med navn Netto og typen LatLng til at være nogle breddegrader og længdegrader. Breddegraderne og længdegraderne er derfor “Netto”’s nye værdi. 

Derefter skaber jeg et objekt som jeg kalder mNetto. mNetto er et objekt af typen Marker og bliver lavet således : 
   private Marker mNetto;

Nu hvor jeg har både en Netto af typen LatLng og en mNetto af typen Marker, er det på tide at jeg får min mNetto til at repræsentere Netto. Dette gør jeg på følgende måde :
 
    // Add a marker
       mNetto = foodmap.addMarker(new MarkerOptions()
               .position(Netto)
               .title("netto"));
       mNetto.setTag(0);

Som du kan se vil mNetto blive sat = med Nettos position. 

Så simpelt er det, man skaber en position giver den en værdi, laver en markør og sætter den til at være den position og så burde det komme frem på kortet når brugeren åbner den. 

