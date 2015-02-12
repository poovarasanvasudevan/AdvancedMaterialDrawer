AdvancedMaterialDrawer
======================

A Material Drawer implementation (Like gMail App)

Based on neokree MaterialDrawer (https://github.com/neokree/MaterialNavigationDrawer)

It requires 11+ API and android support v7 (Toolbar)<br>

### New Features
- HeadItem change animation
- Every HeadTtem can has a own menu
- Menu change animation
- Implement your own on click listener for every section or use the given
- replace bitmap with drawable, so you can use other libs like textDrawable and memory reduce
- and some other options (max drawer width, closeOnHeadItemChange etc.)

### Example APK
https://github.com/madcyph3r/AdvancedMaterialDrawer/raw/master/example-release.apk

### Usage
- For all options see the example (MainActivity)

Your activity (in the Example its the MainActivity)
```java
public class MainActivity extends MaterialNavigationDrawer implements MaterialNavigationDrawerListener {

    MaterialNavigationDrawer drawer = null;

    @Override
    public void init(Bundle savedInstanceState) {

        drawer = this;

        // set drawer menu width in dp
        this.setDrawerDPWidth(300);

        // create HeadItem 1 with menu

        // create menu
        MaterialMenu menu1 = new MaterialMenu();
        // create sections for menu
        MaterialSection section1 = this.newSection("Favoriten", this.getResources().getDrawable(R.drawable.ic_favorite_black_36dp), new FragmentIndex(), false);
        MaterialSection section2 = this.newSection("Verfügbare Tools", this.getResources().getDrawable(R.drawable.ic_list_black_36dp), new FragmentIndex(), false);
        // no new Fragment or Activity. need own listener.
        MaterialSection section3 = this.newSection("Tools Erwerben", this.getResources().getDrawable(R.drawable.ic_extension_black_36dp), false).setSectionColor((Color.parseColor("#ff9800")));
        section3.setNotifications(33);
        // set on click listener for section 3
        section3.setOnClickListener(new MaterialSectionListener() {
            @Override
            public void onClick(MaterialSection section) {
                unSelectOldSection(section);

                // on click action for section 3
                Context context = getApplicationContext();
                CharSequence text = "Tools erwerben toast";
                int duration = Toast.LENGTH_SHORT;

                Toast toast = Toast.makeText(context, text, duration);
                toast.show();

                drawer.closeDrawer();
            }
        });

        Intent intentSettings = new Intent(this, Settings.class);
        // create settings section. with bottom true. it will be shown on the bottom of the drawer
        MaterialSection settingsSection = this.newSection("Settings", this.getResources().getDrawable(R.drawable.ic_settings_black_24dp), intentSettings, true);

        // add sections to the menu
        menu1.getSections().add(section1);
        menu1.getSections().add(section2);
        menu1.getSections().add(section3);
        menu1.getSections().add(new MaterialDevisor());
        menu1.getSections().add(settingsSection);

        // create TextDrawable with lable F and color blue
        TextDrawable headPhoto = TextDrawable.builder()
                .buildRound("F", Color.BLUE);

        // create headItem1 and add menu1
        MaterialHeadItem headItem1 = new MaterialHeadItem("F HeadItem", "F Subtitle", headPhoto, this.getResources().getDrawable(R.drawable.mat5), menu1, 0);
        // add headItem1 to the drawer
        headItem1.setCloseDrawerOnChanged(true);
        this.addHeadItem(headItem1);
        
                // set this class as onchangedListener
        this.setOnChangedListener(this);
    }

    @Override
    public void onBeforeChangedHeadItem(MaterialHeadItem newHeadItem) {
        //Toast.makeText(getApplicationContext(), "on changed", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onAfterChangedHeadItem(MaterialHeadItem newHeadItem) {

    }
```
Do not override the <code>onCreate</code> method in the activity! Use <code>init</code> method instead, it will be called from the library.<br>

Your styles.xml
```xml
<resources>

    <!-- Base application theme. -->
    <style name="MyTheme" parent="MaterialNavigationDrawerTheme">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@android:color/holo_blue_light</item>
        <item name="colorAccent">#FFFFFF</item>
    </style>

</resources>
```

### Import to your Project
For now, you must download the library "materialDrawer.aar" from the directory example/libs and place it in your libs folder.
Change your build.gradle file for your app. 

Add this:
```java
repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:21.0.3'
    compile 'com.amulyakhare:com.amulyakhare.textdrawable:1.0.0'
    //compile project(':materialDrawer')
    compile 'com.nineoldandroids:library:2.4.0'
    compile 'com.daimajia.easing:library:1.0.0@aar'
    compile 'com.daimajia.androidanimations:library:1.1.3@aar'
    compile(name:'materialDrawer', ext:'aar')
}
```

And some changes in your AndroidManifest.xml file are necessary (see comments below. If you copy the manifest file, please remove the comments.) For more information see the example.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="de.madcyph3r.example" >

    <application
        android:allowBackup="true"
        android:icon="@drawable/app_drawer_icon"
        android:label="@string/app_name"
        tools:replace="android:icon,android:theme" <!-- add this, is needed -->
        android:theme="@style/MyTheme" >  <!-- change this to the style.xml name (see before) -->
        <activity
            android:name="de.madcyph3r.example.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name="de.madcyph3r.example.Settings" >

        </activity>
    </application>

</manifest>
```

### Screenshots
<img src="https://github.com/madcyph3r/AdvancedMaterialDrawer/blob/master/Screenshot_1.png" alt="screenshot" width="300px" height="auto" />
<img src="https://github.com/madcyph3r/AdvancedMaterialDrawer/blob/master/Screenshot_2.png" alt="screenshot" width="300px" height="auto" />
<img src="https://github.com/madcyph3r/AdvancedMaterialDrawer/blob/master/Screenshot_3.png" alt="screenshot" width="300px" height="auto" />
