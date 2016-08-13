# Widgets  

clase: WP_Widget  
[Ver documentacion](https://codex.wordpress.org/WordPress_Widgets)

Ejemplo:
``` [php]
<?php

add_action( 'widgets_init', 'register_portfolio_widget' );
function register_portfolio_widget() {
	register_widget( 'Portfolio_Widget' );
}

class Portfolio_Widget extends WP_Widget {
	/**
	 * Sets up the widgets name etc
	 */
	public function __construct() {
		$widget_options = array(
			'description'    => esc_html__( 'Show your most recent projects', 'portfolio-coderhouse' ),
			'title'          => esc_html__( 'Recent Projects' ),
			'limit'          => 10,
			'show_thumbnail' => true,
		);
		parent::__construct( 'portfolio-widget', esc_html__( 'Recent Projects', 'portfolio-coderhouse' ), $widget_options );
	}
	/**
	 * Outputs the options form on admin
	 *
	 * @param array $instance The widget options
	 *
	 * @return void
	 */
	public function form( $instance ) {
		$title          = ! empty( $instance['title'] ) ? $instance['title'] : $this->widget_options['title'];
		$limit          = ! empty( $instance['limit'] ) ? $instance['limit'] : $this->widget_options['limit'];
		$show_thumbnail = ! empty( $instance['show_thumbnail'] ) ? true : false;
		?>

		<p>
			<label for="<?php echo esc_attr( $this->get_field_id( 'title' ) ); ?>"><?php esc_html_e( 'Widget title', 'portfolio-coderhouse' ); ?>:</label><br />
			<input id="<?php echo esc_attr( $this->get_field_id( 'title' ) ); ?>" name="<?php echo esc_attr( $this->get_field_name( 'title' ) ); ?>" type="text" value="<?php echo esc_attr( $title ); ?>">
		</p>
		<p>
			<label for="<?php echo esc_attr( $this->get_field_id( 'limit' ) ); ?>"><?php esc_html_e( 'Number of projects to show', 'portfolio-coderhouse' ); ?></label><br />
			<input id="<?php echo esc_attr( $this->get_field_id( 'limit' ) ); ?>" name="<?php echo esc_attr( $this->get_field_name( 'limit' ) ); ?>" type="text" value="<?php echo esc_attr( $limit ); ?>">
		</p>
		<p>
			<input id="<?php echo esc_attr( $this->get_field_id( 'show_thumbnail' ) ); ?>" name="<?php echo esc_attr( $this->get_field_name( 'show_thumbnail' ) ); ?>" type="checkbox" value="true" <?php checked( $show_thumbnail, true ); ?>>
			<label for="<?php echo esc_attr( $this->get_field_id( 'show_thumbnail' ) ); ?>"><?php esc_html_e( 'Show project thumbnail', 'portfolio-coderhouse' ); ?></label>
		</p>

		<?php
	}
	/**
	 * Processing widget options on save.
	 *
	 * @param array $new_instance The new options
	 * @param array $old_instance The previous options
	 *
	 * @return array
	 */
	public function update( $new_instance, $old_instance ) {
		$instance = array(
			'title'          => isset( $new_instance['title'] ) ? $new_instance['title'] : '',
			'limit'          => isset( $new_instance['limit'] ) ? intval( $new_instance['limit'] ) : 0,
			'show_thumbnail' => isset( $new_instance['show_thumbnail'] ) ? boolval( $new_instance['show_thumbnail'] ) : false,
		);
		return $instance;
	}
	/**
	 * Outputs the content of the widget
	 *
	 * @param array $args
	 * @param array $instance
	 */
	public function widget( $args, $instance ) {
		echo $args['before_widget'];
		if ( ! empty( $instance['title'] ) ) {
			echo $args['before_title'] . apply_filters( 'widget_title', $instance['title'] ) . $args['after_title'];
		}
		$project_query = new WP_Query( array(
				'post_type'      => 'project',
				'posts_per_page' => intval( $instance['limit'] ),
			)
		);
		// The Loop
		if ( $project_query->have_posts() ) : ?>

			<?php while ( $project_query->have_posts() ) : $project_query->the_post(); ?>
				<div class="item">
					<div class="item-inner">
						<?php
							if ( $instance['show_thumbnail'] ) :
								the_post_thumbnail();
							endif;
						?>

						<div class="content">
							<?php the_title( '<h3><a href="' . get_the_permalink() . '">', '</a></h3>' ); ?>
							<div class="description">
								<?php the_excerpt(); ?>
							</div>
						</div>
					</div>
				</div>

			<?php endwhile; ?>

			<?php wp_reset_postdata();
		else :
			echo esc_html__( 'No posts found', 'portfolio-coderhouse' );
		endif;
		echo $args['after_widget'];
	}
}
```
