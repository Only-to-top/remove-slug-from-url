# remove-slug-from-taxonomy

<h2>Удалить слаг кастомного типа записи из URL</h2>

```php
/*
 * Удалить slug из опубликованных постов постоянных ссылок. Только влияет на наш CPT
 */
function sh_remove_cpt_slug( $post_link, $post, $leavename ) {
  if ( in_array( $post->post_type, array( 'post_type_name' ) ) || 'publish' == $post->post_status )
    $post_link = str_replace( '/' . $post->post_type . '/', '/', $post_link ); 
  return $post_link;
}
add_filter( 'post_type_link', 'sh_remove_cpt_slug', 10, 3 );

function sh_parse_request_tricksy( $query ) {
  // Только Цикл основного запроса
  if ( ! $query->is_main_query() ) { return; }
  // Только зациклите наше очень специфическое соответствие правила перезаписи
  if ( 2 != count( $query->query ) || ! isset( $query->query['page'] ) ) return;
  // 'name' will be set if post permalinks are just post_name, otherwise the page rule will match
  if ( ! empty( $query->query['name'] ) ) { $query->set( 'post_type', array( 'post_type_name' ) ); }
}
add_action( 'pre_get_posts', 'sh_parse_request_tricksy' );
```
