# wordpress-notes
A poorly organized collection of Wordpress notes and code


WP Code

<h2><strong>Content Editor Image Fix</strong></h2>
Themes created using our blank template might not include the correct style rules for floating images in the WordPress Content editor, add this code near the top of the stylesheet:
<pre>img.alignright {float: right; margin: 0 0 1em 1em;}
img.alignleft {float: left; margin: 0 1em 1em 0;}
img.aligncenter {display: block; margin-left: auto; margin-right: auto;}
.alignright {float: right;}
.alignleft {float: left;}
.aligncenter {display: block; margin-left: auto; margin-right: auto;}</pre>
<h2>The Loop in Action</h2>
Basic loop to get the content (and title, etc.) of whatever page or post you're currently on
<pre>&lt;?php
   if (have_posts()) : while (have_posts()) : the_post();
        the_title();
        the_content();
   endwhile; endif;
?&gt;</pre>
<h2>Query (loop) through custom post type</h2>
Queries custom post type by name and loops through the content of each. Can also grab the title or any custom fields this way. <strong>wp_reset_query();</strong> restores the $wp_query and global post data to the original main query in case other queries are needed on the page.
<pre>&lt;?php 
     $args = array( 'post_type' =&gt; 'product', 'posts_per_page' =&gt; -1, 'orderby' =&gt; 'menu_order', 'order' =&gt; 'ASC' );
     $loop = new WP_Query( $args ); while ( $loop-&gt;have_posts() ) : $loop-&gt;the_post(); ?&gt;
         -- stuff goes here --
&lt;?php endwhile; wp_reset_query(); ?&gt;
</pre>
<h2>Loop through Custom Post Type by Custom Taxonomies</h2>
<pre class="default prettyprint prettyprinted"><code><span class="pun">&lt;?</span><span class="pln">php
</span><span class="com">// A term is an item of a taxonomy (e.g. "Promotional" could be a term for the taxonomy "video_type")</span>
<span class="com">// ...so $categories could be $terms and it would still make sense</span><span class="pln">
$categories </span><span class="pun">=</span><span class="pln"> get_terms</span><span class="pun">(</span><span class="str">'taxonomy_name'</span><span class="pun">);</span>
<span class="kwd">foreach</span><span class="pun">(</span><span class="pln"> $categories </span><span class="kwd">as</span><span class="pln"> $category </span><span class="pun">):</span>
<span class="pun">?&gt;</span><span class="pln">
  &lt;section class="category-</span><span class="pun">&lt;?</span><span class="pln">php echo $category </span><span class="pun">?&gt;</span><span class="pln">"&gt;
    </span><span class="tag">&lt;h2&gt;</span><span class="pun">&lt;?</span><span class="pln">php echo $category</span><span class="pun">-&gt;</span><span class="pln">name</span><span class="pun">;</span> <span class="com">// Print the cat title </span><span class="pun">?&gt;</span><span class="tag">&lt;/h2&gt;</span>
    <span class="tag">&lt;p</span> <span class="atn">class</span><span class="pun">=</span><span class="atv">"description"</span><span class="tag">&gt;</span><span class="pun">&lt;?</span><span class="pln">php echo $category</span><span class="pun">-&gt;</span><span class="pln">description </span><span class="pun">?&gt;</span><span class="tag">&lt;/p&gt;</span><span class="pln">
    &lt;div class="</span><span class="pun">&lt;?</span><span class="pln">php echo $category</span><span class="pun">-&gt;</span><span class="pln">post_type </span><span class="pun">?&gt;</span><span class="pln">-list"&gt;
      </span><span class="pun">&lt;?</span><span class="pln">php
      </span><span class="com">//select posts in this category (term), and of a specified content type (post type) </span><span class="pln">
      $posts </span><span class="pun">=</span><span class="pln"> get_posts</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span>
        <span class="str">'post_type'</span> <span class="pun">=&gt;</span> <span class="str">'custom_post_type_name'</span><span class="pun">,</span>
        <span class="str">'taxonomy'</span> <span class="pun">=&gt;</span><span class="pln"> $category</span><span class="pun">-&gt;</span><span class="pln">taxonomy</span><span class="pun">,</span>
        <span class="str">'term'</span> <span class="pun">=&gt;</span><span class="pln"> $category</span><span class="pun">-&gt;</span><span class="pln">slug</span><span class="pun">,</span>
        <span class="str">'nopaging'</span> <span class="pun">=&gt;</span> <span class="kwd">true</span><span class="pun">,</span> <span class="com">// to show all posts in this category, could also use 'numberposts' =&gt; -1 instead</span>
      <span class="pun">));</span>
      <span class="kwd">foreach</span><span class="pun">(</span><span class="pln">$posts </span><span class="kwd">as</span><span class="pln"> $post</span><span class="pun">):</span> <span class="com">// begin cycle through posts of this category</span><span class="pln">
        setup_postdata</span><span class="pun">(</span><span class="pln">$post</span><span class="pun">);</span> <span class="com">//set up post data for use in the loop (enables the_title(), etc without specifying a post ID)</span>
      <span class="pun">?&gt;</span><span class="pln">
        // Now you can do things with the post and display it, like so
        &lt;article id="post-</span><span class="pun">&lt;?</span><span class="pln">php the_ID</span><span class="pun">();</span> <span class="pun">?&gt;</span><span class="pln">" </span><span class="pun">&lt;?</span><span class="pln">php post_class</span><span class="pun">();</span> <span class="pun">?&gt;</span><span class="pln">&gt;
          </span><span class="tag">&lt;h3&gt;</span><span class="pun">&lt;?</span><span class="pln">php the_title</span><span class="pun">();</span> <span class="pun">?&gt;</span><span class="tag">&lt;/h3&gt;</span>
          <span class="pun">&lt;?</span><span class="pln">php 
            </span><span class="com">// Getting custom field data example</span><span class="pln">
            echo get_post_meta</span><span class="pun">(</span><span class="pln">$post</span><span class="pun">-&gt;</span><span class="pln">ID</span><span class="pun">,</span> <span class="str">'field_key'</span><span class="pun">,</span> <span class="kwd">true</span><span class="pun">);</span> 
          <span class="pun">?&gt;</span>
          <span class="pun">&lt;?</span><span class="pln">php the_content</span><span class="pun">()</span> <span class="pun">?&gt;</span>
        <span class="tag">&lt;/article&gt;</span>
      <span class="pun">&lt;?</span><span class="pln">php endforeach</span><span class="pun">;</span> <span class="pun">?&gt;</span>
    <span class="tag">&lt;/div&gt;</span>
  <span class="tag">&lt;/section&gt;</span>
<span class="pun">&lt;?</span><span class="pln">php endforeach</span><span class="pun">;</span> <span class="pun">?&gt;</span></code></pre>
<h2>Loop through each custom post in current custom taxonomy</h2>
<pre> &lt;?php
$cat_name = 'portfolio_cat';
 $current_cat_name = get_the_term_list( $post-&gt;ID, $cat_name );
 
 $custom_taxterms = wp_get_object_terms( $post-&gt;ID, $cat_name, array('fields' =&gt; 'ids') );
 
 
 $args = array(
 'post_type' =&gt; 'portfolio',
 'post_status' =&gt; 'publish',
 'posts_per_page' =&gt; -1,
 'orderby' =&gt; 'menu_order',
 'order' =&gt; 'ASC',
 'tax_query' =&gt; array(
 array(
 'taxonomy' =&gt; $cat_name,
 'field' =&gt; 'id',
 'terms' =&gt; $custom_taxterms
 )
 )
 );
 $related_items = new WP_Query( $args );
 if ($related_items-&gt;have_posts()) : while ( $related_items-&gt;have_posts() ) : $related_items-&gt;the_post();
 echo '&lt;a class="here-sone" href="'.get_permalink().'"&gt;&lt;li&gt;'.get_the_title().'&lt;/li&gt;&lt;/a&gt;';
 endwhile; endif; wp_reset_query(); ?&gt;</pre>
<h2>List Categories of Custom Post Type and link to first available post per category</h2>
<pre> &lt;?php 
 $cat_name = 'cat_name';
 $categories = get_terms($cat_name);
 foreach( $categories as $category ):
 
 $the_query = new WP_Query( array( 
 "posts_per_page" =&gt; 1,
 "orderby" =&gt; 'date',
 "order" =&gt; 'DESC',
 "tax_query" =&gt; array(
 array (
 'taxonomy' =&gt; $cat_name, // use the $cat_name you define at the top of your script
 'field' =&gt; 'term_id',
 'terms' =&gt; $category-&gt;term_id, // use the current term in your foreach loop
 ),
 ),
 ) );

 $pageone = get_the_permalink($the_query-&gt;posts[0]);
 
 echo '&lt;li&gt;&lt;a href="'.$pageone.'"&gt;'.$category-&gt;name.'&lt;/a&gt;&lt;/li&gt;';

 endforeach; ?&gt;</pre>
<h2>Query post or page by specific custom field checkbox value</h2>
Will work when querying for pages or by custom post type
<pre>&lt;?php
    $args = array(
        'post_type' =&gt; 'page',
        'posts_per_page' =&gt; -1,
        'post_parent' =&gt; 9,
        'order' =&gt; 'ASC',
        'orderby' =&gt; 'title',
        'meta_query' =&gt; array(
            array(
                'key' =&gt; 'select_box_option',
                'value' =&gt; 'body',
                'compare' =&gt; 'LIKE',
            )
        )
     );
$parent = new WP_Query( $args );
if ($parent-&gt;have_posts()) : while ($parent-&gt;have_posts()) : $parent-&gt;the_post(); ?&gt;
    &lt;li&gt;&lt;a href="&lt;?php the_permalink(); ?&gt;/"&gt;&lt;?php the_title(); ?&gt;&lt;/a&gt;&lt;/li&gt;
&lt;?php endwhile; endif; wp_reset_query(); ?&gt;</pre>

<h2>Create custom link in wp_nav_menu that always links to most recent blog post</h2>
<pre>// Adds a filter to a nav menu item with link to #latestpost. Will always link to the most recent post
// Front end only, don't hack on the settings page
if ( ! is_admin() ) {
    // Hook in early to modify the menu
    // This is before the CSS "selected" classes are calculated
    add_filter( 'wp_get_nav_menu_items', 'replace_placeholder_nav_menu_item_with_latest_post', 10, 3 );
}
 
// Replaces a custom URL placeholder with the URL to the latest post
function replace_placeholder_nav_menu_item_with_latest_post( $items, $menu, $args ) {
 
    // Loop through the menu items looking for placeholder(s)
    foreach ( $items as $item ) {
 
        // Is this the placeholder we're looking for?
        if ( '#latestpost' != $item-&gt;url )
            continue;
 
        // Get the latest post
        $latestpost = get_posts( array(
            'numberposts' =&gt; 1,
        ) );
 
        if ( empty( $latestpost ) )
            continue;
 
        // Replace the placeholder with the real URL
        $item-&gt;url = get_permalink( $latestpost[0]-&gt;ID );
    }
 
    // Return the modified (or maybe unmodified) menu items array
    return $items;
}
</pre>
<h2>Pagination for Child Pages</h2>
The pages loop from the menu order by ascending values, as seen in the <em>get_pages()</em> function.
<pre>&lt;?php
  $pagelist = get_pages("child_of=".$post-&gt;post_parent."&amp;parent=".$post-&gt;post_parent."&amp;sort_column=menu_order&amp;sort_order=asc");
  $pages = array();
  foreach ($pagelist as $page) {
    $pages[] += $page-&gt;ID;
  }

  $current = array_search($post-&gt;ID, $pages);
  $prevID = $pages[$current-1];
  $nextID = $pages[$current+1];
?&gt;

&lt;div class="nextprevbutton "&gt;
&lt;?php if (!empty($prevID)) { ?&gt;
  &lt;div class="previous"&gt;
  &lt;a href="&lt;?php echo get_permalink($prevID); ?&gt;" title="&lt;?php echo get_the_title($prevID); ?&gt;"&gt; &lt; Previous &lt;/a&gt;

&lt;/div&gt;
&lt;?php }
if (!empty($nextID)) { ?&gt;
  &lt;div class="next"&gt;
  &lt;a href="&lt;?php echo get_permalink($nextID); ?&gt;" title="&lt;?php echo get_the_title($nextID); ?&gt;"&gt; Next &gt; &lt;/a&gt;
  &lt;/div&gt;
&lt;?php } ?&gt;
&lt;/div&gt;</pre>
<h2>Pagination for Blog</h2>
<pre>&lt;?php if ( have_posts() ) : ?&gt;

&lt;!-- Add the pagination functions here. --&gt;

&lt;!-- Start of the main loop. --&gt;
&lt;?php while ( have_posts() ) : the_post(); ?&gt;

&lt;!-- the rest of your theme's main loop --&gt;

&lt;?php endwhile; ?&gt;
&lt;!-- End of the main loop --&gt;

&lt;!-- Add the pagination functions here. --&gt;
&lt;div class="nextprevbutton"&gt;
&lt;div class="nav-previous alignleft"&gt;&lt;?php next_posts_link( 'Older posts &gt;' ); ?&gt;&lt;/div&gt;
&lt;div class="nav-next alignright"&gt;&lt;?php previous_posts_link( '&lt; Newer posts' ); ?&gt;&lt;/div&gt;

&lt;?php else : ?&gt;
&lt;p&gt;&lt;?php _e('Sorry, no posts matched your criteria.'); ?&gt;&lt;/p&gt;
&lt;?php endif; ?&gt;

</pre>
<h2>Create infinite pagination of child pages</h2>
This script goes into function.php. When
<pre>dbdb_next_page_link()</pre>
and
<pre>dbdb_prev_page_link()</pre>
are called it will create pagination of the child pages you are currently on. Adjust $output to style it as desired
<pre>/*----------------------child page navigation--------------------------*/
function relative_value_array($array, $current_val = '', $offset = 1) {
    $values = array_values($array);
    $current_val_index = array_search($current_val, $values); 
    if( isset($values[$current_val_index + $offset]) ) {
        return $values[$current_val_index + $offset];
    }
    return false;
};


function first_array_value($array, $current_val = '', $offset = 1) {
    $values = array_values($array);
    if( isset($values[0 + $offset]) ) {
        return $values[0 + $offset];
    }
    return false;
};

function last_array_value($array, $current_val = '', $offset = 1) {
    $values = array_values($array);
    if( isset($values[count($array) + $offset]) ) {
        return $values[count($array) + $offset];
    }
    return false;
};

function dbdb_prev_page_link() {
    global $post;
    if ( isset($post-&gt;post_parent) &amp;&amp; $post-&gt;post_parent &gt; 0 ) {

        $children = get_pages('&amp;sort_column=post_date&amp;sort_order=asc&amp;child_of='.$post-&gt;post_parent.'&amp;parent='.$post-&gt;post_parent);

    };
    foreach( $children as $child ) { $child_id_array[] = $child-&gt;ID; }
    $prev_page_id = relative_value_array($child_id_array, $post-&gt;ID, -1);
    $output = '';
    if( '' != $prev_page_id ) {
        $output .= '<a href="' . get_page_link($prev_page_id) . '"> « '. Previous . '</a>';
    }
		else {
				$prev_page_id = last_array_value($child_id_array, $post-&gt;ID, -1 );
        $output .= '<a href="' . get_page_link($prev_page_id) . '"> « '. Previous . '</a>';
    }
    return $output;
};
function dbdb_next_page_link() {
    global $post;
    if ( isset($post-&gt;post_parent) &amp;&amp; $post-&gt;post_parent &gt; 0 ) {

        $children = get_pages('&amp;sort_column=post_date&amp;sort_order=asc&amp;child_of='.$post-&gt;post_parent.'&amp;parent='.$post-&gt;post_parent);
    };
    foreach( $children as $child ) { $child_id_array[] = $child-&gt;ID; }
    $next_page_id = relative_value_array($child_id_array, $post-&gt;ID, 1);
    $output = '';
    if( '' != $next_page_id ) {
        $output .= '<a href="' . get_page_link($next_page_id) . '">'. Next . ' »</a>';
    } else{
			$next_page_id = first_array_value($child_id_array, $post-&gt;ID, 0);
	 $output .= '<a href="' . get_page_link($next_page_id) . '">'. Next . ' »</a>';
    }
    return $output;
};

</pre>
<h2>Redirect Parent Page to First Child Page</h2>
Goes to the first child by menu order as directed in the <em>get_pages()</em>. Adds a 301 status code to prevent SEO issues.
<pre>$pagekids = get_pages("child_of=".$post-&gt;ID."&amp;sort_column=menu_order");
if ($pagekids) {
  $firstchild = $pagekids[0];
  wp_redirect(get_permalink($firstchild-&gt;ID), 301);
}</pre>
<h2>Dynamically Load List of Child Pages</h2>
Makes an <em>unordered list </em>of child pages. In this code, the list is sorted by menu order.
<pre> &lt;!-- Dynamically load list of services added --&gt;
 &lt;?php
 if ( $post-&gt;post_parent ) {
    $children = wp_list_pages( array(
    'title_li' =&gt; '',
    'child_of' =&gt; $post-&gt;post_parent,
    'echo' =&gt; 0,
    'sort_column' =&gt; 'menu_order'
 ) );
 } 
else {
    $children = wp_list_pages( array(
    'title_li' =&gt; '',
    'child_of' =&gt; $post-&gt;ID,
    'echo' =&gt; 0,
    'sort_column' =&gt; 'menu_order'
 ) );
 }
 
 if ( $children ) : ?&gt;
   &lt;ul class="option-list"&gt;
     &lt;?php echo $children; ?&gt;
   &lt;/ul&gt;
 &lt;?php endif; ?&gt;
 &lt;!-- --&gt;
</pre>
<h2>Apply a 'menu_order' filter to Prev/Next links in a post type single.php</h2>
Usually the following code will create prev/next links that go to the prev/next post in chronological order
<pre>&lt;?php previous_post_link( '%link ','Previous' ); ?&gt;
&lt;?php next_post_link('%link','Next' ); ?&gt;</pre>
The following code will apply a meta filter that will prev/next to the correct pages that are sorted by 'menu_order'. You should be able to modify this to order them by other things, such as 'title', as well. Insert the filter before you call the prev/next_post_link functions. You can also search by 'p.post_parent = number' in the query
<pre>function my_previous_post_where() {
	global $post, $wpdb;
	return $wpdb-&gt;prepare( "WHERE p.menu_order &lt; %s AND p.post_type = %s AND p.post_status = 'publish'", $post-&gt;menu_order, $post-&gt;post_type);
}
add_filter( 'get_previous_post_where', 'my_previous_post_where' );

function my_next_post_where() {
	global $post, $wpdb;
	return $wpdb-&gt;prepare( "WHERE p.menu_order &gt; %s AND p.post_type = %s AND p.post_status = 'publish'", $post-&gt;menu_order, $post-&gt;post_type);
}
add_filter( 'get_next_post_where', 'my_next_post_where' );

function my_previous_post_sort() {
	return "ORDER BY p.menu_order desc LIMIT 1";
}
add_filter( 'get_previous_post_sort', 'my_previous_post_sort' );

function my_next_post_sort() {
	return "ORDER BY p.menu_order asc LIMIT 1";
}
add_filter( 'get_next_post_sort', 'my_next_post_sort' );
</pre>
<h2>Get alt tag from featured image</h2>
<pre> echo get_post_meta(get_post_thumbnail_id($post-&gt;ID),'_wp_attachment_image_alt', true);</pre>





JS jQuery code

<h2>Jquery slide animate to anchor</h2>
Handy bit of Jquery code because it uses a smooth animation automatically on page load and also automatically gets the id to scroll to by pulling it from the hash in the URL. Looks like it requires a plugin (jquery.easing.min.js) for that easing option, but any normal easing option can be used instead
<pre>
var window_hash = window.location.hash;
if(window_hash.length > 0){
   window_hash = window_hash.slice(0,-1);
	$('html, body').stop().animate({
		scrollTop: ($(window_hash).offset().top - 90)
	}, 1250, 'easeInOutExpo');
}
</pre>


Page Optimization

<h2>What server is the domain on?</h2>
To find this out go to <a href="https://tools.pingdom.com/" target="_blank">tools.pingdom.com</a> and enter the domain. Then look into the dropdown on the domain info. In the picture below the <strong>Server</strong> is <strong>Apache</strong>.

<img class="img-responsive alignnone wp-image-15 size-full" src="http://projects.affordableimage.com/webteam/wp-content/uploads/2016/09/pingdom-looking-for-server.png" alt="pingdom-looking-for-server" width="571" height="282" />
<h2 id="leverage-cache">Leverage Caching</h2>
To leverage caching add this script to the top of the <strong>.htaccess</strong> file
<pre>## EXPIRES CACHING ##
&lt;IfModule mod_expires.c&gt;
ExpiresActive On
ExpiresByType image/jpg "access 1 year"
ExpiresByType image/jpeg "access 1 year"
ExpiresByType image/gif "access 1 year"
ExpiresByType image/png "access 1 year"
ExpiresByType text/css "access 1 month"
ExpiresByType text/html "access 1 month"
ExpiresByType application/pdf "access 1 month"
ExpiresByType text/x-javascript "access 1 month"
ExpiresByType application/x-shockwave-flash "access 1 month"
ExpiresByType image/x-icon "access 1 year"
ExpiresDefault "access 1 month"
&lt;/IfModule&gt;
## EXPIRES CACHING ##</pre>
If that doesn't work try
<pre> &lt;filesMatch ".(ico|pdf|flv|jpg|svg|jpeg|png|gif|js|css|swf)$"&gt;
   Header set Cache-Control "max-age=84600, public"
 &lt;/filesMatch&gt;
</pre>
If the domain is on a <strong>NGINX Server</strong>
<pre>location ~* \.(js|css|png|jpg|svg|jpeg|gif|ico)$ {
 expires 2d;
 add_header Cache-Control "public, no-transform";
}</pre>
Or if the domain is on <strong>ASP.NET Server</strong>, look into the ftp files for <strong>web.config</strong> and add this script
<pre>&lt;system.webServer&gt;
 &lt;staticContent&gt;
  &lt;clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="7.00:00:00" /&gt;
 &lt;/staticContent&gt;
&lt;/system.webServer&gt;</pre>
<h2>GZIP Compression</h2>
Try out the scipts in the <strong>.htaccess</strong>. First try the next code.
<pre>&lt;ifModule mod_gzip.c&gt;
mod_gzip_on Yes
mod_gzip_dechunk Yes
mod_gzip_item_include file .(html?|txt|css|js|php|pl)$
mod_gzip_item_include handler ^cgi-script$
mod_gzip_item_include mime ^text/.*
mod_gzip_item_include mime ^application/x-javascript.*
mod_gzip_item_exclude mime ^image/.*
mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
&lt;/ifModule&gt;</pre>
For an <strong>Apache Server</strong>
<pre>## GZIP ##
&lt;IfModule mod_deflate.c&gt;
 # Compress HTML, CSS, JavaScript, Text, XML and fonts
 AddOutputFilterByType DEFLATE application/javascript
 AddOutputFilterByType DEFLATE application/rss+xml
 AddOutputFilterByType DEFLATE application/vnd.ms-fontobject
 AddOutputFilterByType DEFLATE application/x-font
 AddOutputFilterByType DEFLATE application/x-font-opentype
 AddOutputFilterByType DEFLATE application/x-font-otf
 AddOutputFilterByType DEFLATE application/x-font-truetype
 AddOutputFilterByType DEFLATE application/x-font-ttf
 AddOutputFilterByType DEFLATE application/x-javascript
 AddOutputFilterByType DEFLATE application/xhtml+xml
 AddOutputFilterByType DEFLATE application/xml
 AddOutputFilterByType DEFLATE font/opentype
 AddOutputFilterByType DEFLATE font/otf
 AddOutputFilterByType DEFLATE font/ttf
 AddOutputFilterByType DEFLATE image/svg+xml
 AddOutputFilterByType DEFLATE image/x-icon
 AddOutputFilterByType DEFLATE text/css
 AddOutputFilterByType DEFLATE text/html
 AddOutputFilterByType DEFLATE text/javascript
 AddOutputFilterByType DEFLATE text/plain
 AddOutputFilterByType DEFLATE text/xml

 # Remove browser bugs (only needed for really old browsers)
 BrowserMatch ^Mozilla/4 gzip-only-text/html
 BrowserMatch ^Mozilla/4\.0[678] no-gzip
 BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
 Header append Vary User-Agent
&lt;/IfModule&gt;
## GZIP ##</pre>
For <strong>NGINX Server</strong>, you can enable compression by adding the following to your <strong>nginx.conf</strong> file.
<pre>gzip on;
gzip_comp_level 2;
gzip_http_version 1.0;
gzip_proxied any;
gzip_min_length 1100;
gzip_buffers 16 8k;
gzip_types text/plain text/html text/css application/x-javascript text/xml application/xml application/xml+rss text/javascript;
gzip_disable "MSIE [1-6].(?!.*SV1)";
gzip_vary on;</pre>
Here is the full example web.config file
<pre>&lt;?xml version="1.0"?&gt;
&lt;configuration&gt;

 &lt;system.web&gt;
   &lt;compilation debug="true" targetFramework="4.0"/&gt;
 &lt;/system.web&gt;
 
 &lt;system.webServer&gt;
   &lt;httpErrors errorMode="Custom"&gt;
     &lt;clear/&gt;
     &lt;error statusCode="404" path="404.html" responseMode="Redirect"/&gt;
   &lt;/httpErrors&gt;
 &lt;/system.webServer&gt;
 
 &lt;system.webServer&gt;
    &lt;staticContent&gt;
      &lt;clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="7.00:00:00" /&gt;
    &lt;/staticContent&gt;
 &lt;/system.webServer&gt;
 
&lt;/configuration&gt;</pre>
<h2>Remove Query String From Static Resources</h2>
Enter at the end of <strong>functions.php </strong>in your WordPress files.
<pre>/*** Remove Query String from Static Resources ***/
function _remove_script_version( $src ){
 $parts = explode( '?ver', $src );
 return $parts[0];
}
add_filter( 'script_loader_src', '_remove_script_version', 15, 1 );
add_filter( 'style_loader_src', '_remove_script_version', 15, 1 );</pre>
<h2>Parallelize Downloads Across Hostnames</h2>
First try to minify and combine javascript and css files first before trying out this script. Add code to the end of <strong>functions.php</strong> , read the comments of the code as well, there are instructions.
<pre>// Parallelize downloads across hostnames

function parallelize_hostnames($url, $id) {
 $hostname = par_get_hostname($url); //call supplemental function
 $url = str_replace(parse_url(get_bloginfo('url'), PHP_URL_HOST), $hostname, $url);
 return $url;
}
function par_get_hostname($name) {
 //add your subdomains here, as many as you want. ********** CHANGE DOMAIN **********
 $subdomains = array('media1.mydomain.com','media2.mydomain.com'); 
 $host = abs(crc32(basename($name)) % count($subdomains));
 $hostname = $subdomains[$host];
 return $hostname;
}
add_filter('wp_get_attachment_url', 'parallelize_hostnames', 10, 2);</pre>
<h2>Moving scripts to the footer</h2>

SEO

<h2>Making a 5 star review link for Google</h2>
Get google maps link.
<blockquote>https://www.google.com/maps/place/Dr.+Julie+Phillips+Prosthodontics/</blockquote>
Grab the search query (text with plus signs)
<blockquote>Dr.+Julie+Phillips+Prosthodontics</blockquote>
"View Source" of the Google Maps page and search for "ludocid" (Ctrl-F) on the source code. Here is an example what you will find.
<blockquote>ludocid\\u003d10332763409006310141 #lrd\\u003d0x88531ed2d17eac7b: 0x8f6559933df9b2fd,2
<strong>ludocid number is after the "d"</strong> = ("10332763409006310141")
<strong>lrd number is the same, but nothing after the comma</strong> =("0x88531ed2d17eac7b:0x8f6559933df9b2fd")</blockquote>
Add your numbers into the new search query url
<blockquote>https://www.google.com/search?q=(<strong>Add the search text here</strong>)&amp;ludocid=(<strong>ludocid number here</strong>)#lrd=(<strong>lrd number here</strong>),3,5</blockquote>
The url will now look like this, and a five star review will populate the page.
<blockquote>https://www.google.com/search?q=Dr.+Julie+Phillips+Prosthodontics&amp;  ludocid=10332763409006310141# lrd=0x88531ed2d17eac7b: 0x8f6559933df9b2fd,3,5</blockquote>

<h2>SEO Issues</h2>
Canonical Link issue. Two pages that are from the same page with different links. Add this link tag inside "head" tag before the &lt;link rel="stylesheet" /&gt;
<blockquote>&lt;link rel="canonical" href="http://example.com/" /&gt;</blockquote>
&nbsp;
<h2>Add Event Tracking for Google Analytics</h2>
Add the following script to the header file.

&lt;script&gt;
var trackOutboundLink = function(url) {
ga('send', 'event', 'outbound', 'click', url, {
'transport': 'beacon'
});
}
&lt;/script&gt;

&nbsp;

Now add the following code to each a tag or link and label them appropriately

onclick="trackOutboundLink('Request-an-appointment-header');"

&nbsp;

Remember to label the even appropriately, if it is linking to Facebook in the header section of the website an example is:

&lt;a href="https://www.facebook.com/Athena-Skin-Body-1840833106242156/" target="_blank" onclick="trackOutboundLink('facebook-header');"&gt;Facebook&lt;/a&gt;
<pre>
&lt;?php function add_this_script_footer(){ ?&gt;

&lt;script src="&lt;?php bloginfo('template_directory'); ?&gt;/script.js"&gt;&lt;/script&gt; &lt;!-- bootstap, script, lightslider --&gt;

&lt;?php } add_action('wp_footer', 'add_this_script_footer'); ?&gt;
</pre>



WP Plugins
<h2>Above the Fold Optimization</h2>
<a href="https://wordpress.org/plugins/above-the-fold-optimization/" target="_blank">https://wordpress.org/plugins/above-the-fold-optimization/</a>

CSS/JS delivery optimization. Will provide async loading of certain resources to help prevent render-blocking files from blocking content.
<h2>Advanced Excerpt</h2>
<a href="https://wordpress.org/plugins/advanced-excerpt/">https://wordpress.org/plugins/advanced-excerpt/</a>

Easy controls for blog index page excerpts. Preserves HTML tags like images and iframes and cuts off blog posts when they reach a certain number of characters/words to keep index pages from getting too long. Can add and edit a "Read More" link.

Found a bit of code that disables the "Read More" link if the post isn't actually excerpted (short enough to just display the whole thing). Go into the plugin editor to the <span style="color: #008000;">advanced-excerpt/functions/functions.php </span>file and paste into the very bottom:
<pre>function ca_only_add_more_when_required( $default, $text, $options ) {
 $text = strip_tags( $text );
 if ( 'words' === $options['length_type'] ) {
 $excerpt_length = str_word_count( $text );
 } else {
 $excerpt_length = strlen( $text );
 }
 if ( $excerpt_length &gt; $options['length'] ) return $default;
 return true;
}
add_filter( 'advanced_excerpt_disable_add_more', 'ca_only_add_more_when_required', 10, 3 );</pre>
