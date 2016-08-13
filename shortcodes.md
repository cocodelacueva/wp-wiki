# Shortcodes

Shortcodes and Transients ejemplos:

``` [php]
<?php
/**
 * Manage shortcodes.
 *
 * @package     Portfolio_Coderhouse
 * @author      CoderHouse <hello@coderhouse.com>
 * @license     GPL-2.0+
 * @link        http://www.coderhouse.com/
 * @copyright   2016 CoderHouse
 * @since       1.0
 */
// If this file is called directly, abort.
if ( ! defined( 'WPINC' ) ) {
	die;
}
add_shortcode( 'portfolio', 'portfolio_shortcode' );
/**
 * Process [portfolio] shortcode.
 *
 * @param  array  $atts
 *
 * @return string
 */
function portfolio_shortcode( $atts ) {
	$atts = shortcode_atts( array(
			'columns' => 4,
		), $atts
	);
	if ( ! ( $content = get_transient( 'portfolio_shortcode_columns_' . $atts['columns'] ) ) ) {
		// The Query
		$project_query = new WP_Query( array(
				'post_type' => 'project',
			)
		);
		$columns = intval( $atts['columns'] );
		// Limit columns between 1 and 6.
		if ( $columns < 1 ) {
			$columns = 1;
		} elseif ( $columns > 6 ) {
			$columns = 6;
		}
		ob_start();
		// The Loop
		if ( $project_query->have_posts() ) : ?>

			<div class="grid portfolio-grid columns-<?php echo esc_attr( $columns ); ?>">

				<?php while ( $project_query->have_posts() ) : $project_query->the_post(); ?>
					<div class="item">
						<div class="item-inner">
							<?php the_post_thumbnail(); ?>

							<div class="content">
								<?php the_title( '<h3><a href="' . get_the_permalink() . '">', '</a></h3>' ); ?>
							</div>
						</div>
					</div>
				<?php endwhile; ?>

			</div>

			<?php wp_reset_postdata();
		else :
			echo esc_html__( 'No projects found', 'portfolio-coderhouse' );
		endif;
		$content = ob_get_clean();
		set_transient( 'portfolio_shortcode_columns_' . $atts['columns'], $content, ( 60 * 60 * 24 ) );
	}
	return $content;
}
if ( ! function_exists( 'portfolio_delete_shortcode_transients' ) ) :
add_action( 'save_post', 'portfolio_delete_shortcode_transients' );
/**
 * Delete shortcode transients.
 *
 * @since 1.0
 */
function portfolio_delete_shortcode_transients() {
	for ( $i = 1; $i <= 6; $i++ ) {
		delete_transient( 'portfolio_shortcode_columns_' . $i );
	}
}
endif;
```
