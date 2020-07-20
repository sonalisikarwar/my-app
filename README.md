/*activity_main.xml*/
Notseas<?xNotseasml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"></ListView>
</RelativeLayout>


/*mainActivity.java*/

package com.example.immunity;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

public class MainActivity extends AppCompatActivity {

    ListView listView;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate( savedInstanceState );
        setContentView( R.layout.activity_main );

        String[] tS = getResources().getStringArray( R.array.title_s );
        final String[] dS = getResources().getStringArray( R.array.details_s );

        listView = findViewById( R.id.list );
        ArrayAdapter<String> adapter = new ArrayAdapter<String>( this,
                android.R.layout.simple_list_item_1, tS );
        listView.setAdapter( adapter );

        listView.setOnItemClickListener( new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                String t = dS[position];
                Intent intent = new Intent( MainActivity.this, MainActivity2.class );
                intent.putExtra( "s", t );
                startActivity( intent );
            }
        } );

    }}

/*Activitymain2.xml*/

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp"
    tools:context=".MainActivity2">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/txt"
            android:layout_width="10dp"
            android:layout_height="match_parent"
            android:layout_widht="match_parent"
            android:gravity="center"
            android:text="text"
            android:textSize="20sp">

        </TextView>

    </ScrollView>

</RelativeLayout>

/*MainActivity2.java*/

package com.example.immunity;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;

public class MainActivity2 extends AppCompatActivity {

    TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        textView= findViewById(R.id.text);
        String ds =getIntent().getStringExtra( "s");
    }
}
/*
 * Copyright (C) 2015 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package androidx.appcompat.app;

import android.content.Context;
import android.content.Intent;
import android.content.res.Configuration;
import android.content.res.Resources;
import android.os.Build;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.view.KeyEvent;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.Window;

import androidx.annotation.CallSuper;
import androidx.annotation.ContentView;
import androidx.annotation.IdRes;
import androidx.annotation.LayoutRes;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.annotation.StyleRes;
import androidx.appcompat.app.AppCompatDelegate.NightMode;
import androidx.appcompat.view.ActionMode;
import androidx.appcompat.widget.Toolbar;
import androidx.appcompat.widget.VectorEnabledTintResources;
import androidx.core.app.ActivityCompat;
import androidx.core.app.NavUtils;
import androidx.core.app.TaskStackBuilder;
import androidx.fragment.app.FragmentActivity;


public class AppCompatActivity extends FragmentActivity implements AppCompatCallback,
        TaskStackBuilder.SupportParentable, ActionBarDrawerToggle.DelegateProvider {

    private AppCompatDelegate mDelegate;
    private Resources mResources;

    
    public AppCompatActivity() {
        super();
    }

     @ContentView
    public AppCompatActivity(@LayoutRes int contentLayoutId) {
        super(contentLayoutId);
    }

    @Override
    protected void attachBaseContext(Context newBase) {
        super.attachBaseContext(newBase);
        getDelegate().attachBaseContext(newBase);
    }

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        final AppCompatDelegate delegate = getDelegate();
        delegate.installViewFactory();
        delegate.onCreate(savedInstanceState);
        super.onCreate(savedInstanceState);
    }

    @Override
    public void setTheme(@StyleRes final int resId) {
        super.setTheme(resId);
        getDelegate().setTheme(resId);
    }

    @Override
    protected void onPostCreate(@Nullable Bundle savedInstanceState) {
        super.onPostCreate(savedInstanceState);
        getDelegate().onPostCreate(savedInstanceState);
    }

   
    @Nullable
    public ActionBar getSupportActionBar() {
        return getDelegate().getSupportActionBar();
    }

   
 
    public void setSupportActionBar(@Nullable Toolbar toolbar) {
        getDelegate().setSupportActionBar(toolbar);
    }

    @NonNull
    @Override
    public MenuInflater getMenuInflater() {
        return getDelegate().getMenuInflater();
    }

    @Override
    public void setContentView(@LayoutRes int layoutResID) {
        getDelegate().setContentView(layoutResID);
    }

    @Override
    public void setContentView(View view) {
        getDelegate().setContentView(view);
    }

    @Override
    public void setContentView(View view, ViewGroup.LayoutParams params) {
        getDelegate().setContentView(view, params);
    }

    @Override
    public void addContentView(View view, ViewGroup.LayoutParams params) {
        getDelegate().addContentView(view, params);
    }

    @Override
    public void onConfigurationChanged(@NonNull Configuration newConfig) {
        super.onConfigurationChanged(newConfig);

        if (mResources != null) {
            // The real (and thus managed) resources object was already updated
            // by ResourcesManager, so pull the current metrics from there.
            final DisplayMetrics newMetrics = super.getResources().getDisplayMetrics();
            mResources.updateConfiguration(newConfig, newMetrics);
        }

        getDelegate().onConfigurationChanged(newConfig);
    }

    @Override
    protected void onPostResume() {
        super.onPostResume();
        getDelegate().onPostResume();
    }

    @Override
    protected void onStart() {
        super.onStart();
        getDelegate().onStart();
    }

    @Override
    protected void onStop() {
        super.onStop();
        getDelegate().onStop();
    }

    @SuppressWarnings("TypeParameterUnusedInFormals")
    @Override
    public <T extends View> T findViewById(@IdRes int id) {
        return getDelegate().findViewById(id);
    }

    @Override
    public final boolean onMenuItemSelected(int featureId, @NonNull android.view.MenuItem item) {
        if (super.onMenuItemSelected(featureId, item)) {
            return true;
        }

        final ActionBar ab = getSupportActionBar();
        if (item.getItemId() == android.R.id.home && ab != null &&
                (ab.getDisplayOptions() & ActionBar.DISPLAY_HOME_AS_UP) != 0) {
            return onSupportNavigateUp();
        }
        return false;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        getDelegate().onDestroy();
    }

    @Override
    protected void onTitleChanged(CharSequence title, int color) {
        super.onTitleChanged(title, color);
        getDelegate().setTitle(title);
    }

   
    public boolean supportRequestWindowFeature(int featureId) {
        return getDelegate().requestWindowFeature(featureId);
    }

    @Override
    public void supportInvalidateOptionsMenu() {
        getDelegate().invalidateOptionsMenu();
    }

    @Override
    public void invalidateOptionsMenu() {
        getDelegate().invalidateOptionsMenu();
    }

   
    @Override
    @CallSuper
    public void onSupportActionModeStarted(@NonNull ActionMode mode) {
    }

    
    @Override
    @CallSuper
    public void onSupportActionModeFinished(@NonNull ActionMode mode) {
    }

    
    @Nullable
    @Override
    public ActionMode onWindowStartingSupportActionMode(@NonNull ActionMode.Callback callback) {
        return null;
    }

    
    @Nullable
    public ActionMode startSupportActionMode(@NonNull ActionMode.Callback callback) {
        return getDelegate().startSupportActionMode(callback);
    }

   
    @Deprecated
    public void setSupportProgressBarVisibility(boolean visible) {
    }

    
    @Deprecated
    public void setSupportProgressBarIndeterminateVisibility(boolean visible) {
    }

   
    @Deprecated
    public void setSupportProgressBarIndeterminate(boolean indeterminate) {
    }

  
    @Deprecated
    public void setSupportProgress(int progress) {
    }

    
    public void onCreateSupportNavigateUpTaskStack(@NonNull TaskStackBuilder builder) {
        builder.addParentStack(this);
    }

    
    public void onPrepareSupportNavigateUpTaskStack(@NonNull TaskStackBuilder builder) {
    }

    
    public boolean onSupportNavigateUp() {
        Intent upIntent = getSupportParentActivityIntent();

        if (upIntent != null) {
            if (supportShouldUpRecreateTask(upIntent)) {
                TaskStackBuilder b = TaskStackBuilder.create(this);
                onCreateSupportNavigateUpTaskStack(b);
                onPrepareSupportNavigateUpTaskStack(b);
                b.startActivities();

                try {
                    ActivityCompat.finishAffinity(this);
                } catch (IllegalStateException e) {
                    // This can only happen on 4.1+, when we don't have a parent or a result set.
                    // In that case we should just finish().
                    finish();
                }
            } else {
                // This activity is part of the application's task, so simply
                // navigate up to the hierarchical parent activity.
                supportNavigateUpTo(upIntent);
            }
            return true;
        }
        return false;
    }

 
    @Nullable
    @Override
    public Intent getSupportParentActivityIntent() {
        return NavUtils.getParentActivityIntent(this);
    }

    
    public boolean supportShouldUpRecreateTask(@NonNull Intent targetIntent) {
        return NavUtils.shouldUpRecreateTask(this, targetIntent);
    }

    
    
    public void supportNavigateUpTo(@NonNull Intent upIntent) {
        NavUtils.navigateUpTo(this, upIntent);
    }

    @Override
    public void onContentChanged() {
        // Call onSupportContentChanged() for legacy reasons
        onSupportContentChanged();
    }

   
    @Deprecated
    public void onSupportContentChanged() {
    }

    @Nullable
    @Override
    public ActionBarDrawerToggle.Delegate getDrawerToggleDelegate() {
        return getDelegate().getDrawerToggleDelegate();
    }

   
    @Override
    public boolean onMenuOpened(int featureId, Menu menu) {
        return super.onMenuOpened(featureId, menu);
    }

   
     
    @Override
    public void onPanelClosed(int featureId, @NonNull Menu menu) {
        super.onPanelClosed(featureId, menu);
    }

    @Override
    protected void onSaveInstanceState(@NonNull Bundle outState) {
        super.onSaveInstanceState(outState);
        getDelegate().onSaveInstanceState(outState);
    }

   
    @NonNull
    public AppCompatDelegate getDelegate() {
        if (mDelegate == null) {
            mDelegate = AppCompatDelegate.create(this, this);
        }
        return mDelegate;
    }

    @Override
    public boolean dispatchKeyEvent(KeyEvent event) {
        // Let support action bars open menus in response to the menu key prioritized over
        // the window handling it
        final int keyCode = event.getKeyCode();
        final ActionBar actionBar = getSupportActionBar();
        if (keyCode == KeyEvent.KEYCODE_MENU
                && actionBar != null && actionBar.onMenuKeyEvent(event)) {
            return true;
        }
        return super.dispatchKeyEvent(event);
    }

    @Override
    public Resources getResources() {
        if (mResources == null && VectorEnabledTintResources.shouldBeUsed()) {
            mResources = new VectorEnabledTintResources(this, super.getResources());
        }
        return mResources == null ? super.getResources() : mResources;
    }
    private boolean performMenuItemShortcut(int keycode, KeyEvent event) {
        if (!(Build.VERSION.SDK_INT >= 26) && !event.isCtrlPressed()
                && !KeyEvent.metaStateHasNoModifiers(event.getMetaState())
                && event.getRepeatCount() == 0
                && !KeyEvent.isModifierKey(event.getKeyCode())) {
            final Window currentWindow = getWindow();
            if (currentWindow != null && currentWindow.getDecorView() != null) {
                final View decorView = currentWindow.getDecorView();
                if (decorView.dispatchKeyShortcutEvent(event)) {
                    return true;
                }
            }
        }
        return false;
    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (performMenuItemShortcut(keyCode, event)) {
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }

    @Override
    public void openOptionsMenu() {
        ActionBar actionBar = getSupportActionBar();
        if (getWindow().hasFeature(Window.FEATURE_OPTIONS_PANEL)
                && (actionBar == null || !actionBar.openOptionsMenu())) {
            super.openOptionsMenu();
        }
    }

    @Override
    public void closeOptionsMenu() {
        ActionBar actionBar = getSupportActionBar();
        if (getWindow().hasFeature(Window.FEATURE_OPTIONS_PANEL)
                && (actionBar == null || !actionBar.closeOptionsMenu())) {
            super.closeOptionsMenu();
        }
    }
    protected void onNightModeChanged(@NightMode int mode) {
    }
}
    /*strings.xml*/

   
<resources>
    <string name="app_name">immunity</string>
    <string-array name="title_s">
        <item>FOOD</item>
        <item>Hearbs</item>
        <item>Pranayama</item>
        <item>Friuts</item>
        <item>Nuts</item>
        <item>Exercise</item>
        <item>Indian spices</item>
        <item>Water intake</item>
        <item>Yoga</item>
        <item>Meditation</item>
        <item>Raw vegetables</item>
        <item>Mental health</item>
        <item>Good sleep</item>
        <item>Learn new things</item>
        <item>Give time to your hobbies</item>
        <item>Vitamin D</item>


    </string-array>

    <string-array name="details_s">
        <item>Food: Sprouts,coconut and wholewheat</item>
        <item>Herbs: Tulsi</item>
        <item>Pranayama: Breathing exercise breathe in hold and breathe out</item>
        <item>Friuts: Mainly Citrus friuts like Orange, lemon and Amla </item>
        <item>Nuts: Kaju , badam and kishmish</item>
        <item>Exercise: Daily exercise is necessary for immunity and walk </item>
        <item>Indian spices: Kaali mirch,haldi ,sauth and Daalchini</item>
        <item>Water intake: Drink minimum 12 glasses of water daily </item>
        <item>Yoga: Anulom-vilom, bhramari,bhujangasan and Shirshasan</item>
        <item>Meditation: Daily 20 min meditation keeps you calm </item>
        <item>Raw vegetables: Salad and eating vegetables raw as compared to fried is better.But clean before use </item>

        <item>Mental health: Keep smile on your face.Listen music and spend some with yourself.Writing and talking with freinds makes you happy.</item>

        <item>Good sleep: Atleast 8 hours of sleep needed fro our body to rest.</item>
        <item>Learn new things: It will update you from growing world</item>
        <item>Vitamin D: Vitamin D is also necessary for bones and health.</item>
        <item>story15</item>
    </string-array>
</resources>
