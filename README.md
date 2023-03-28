# Display Custom Posts inside Tab element with custom taxonomy.

![Foo](https://i.imgur.com/i1gU3AS.png)

``` <?php
$args = array(
	'orderby' => 'ID'
);
$terms = get_terms( 'service_cat', $args );

?>

<!-- bootstrap tabs -->
<nav>
    <div class="nav nav-tabs mb-3" id="nav-tab" role="tablist">
	<?php
        $count = 0;
        foreach ( $terms as $term ) : $count ++; ?>
            <button class="nav-link <?php if ( $count == 1 ) echo 'active' ?>" id="<?php echo $term->slug; ?>-tab" data-bs-toggle="tab" data-bs-target="#<?php echo $term->slug; ?>" type="button" role="tab" aria-controls="<?php echo $term->slug; ?>" aria-selected="true"><?php echo $term->name; ?></button>
            <?php
        endforeach;	?>
    </div>
</ul>

<div class="tab-content p-3 border bg-light" id="nav-tabContent">
	<?php
	$count = 0;
	foreach ( $terms as $term ) : $count ++;

    ?>
		<div class="tab-pane <?php if ( $count == 1 ) echo 'active' ?>" id="<?php echo $term->slug ?>">
			<?php
			$args = array(
				'post_type' => 'services',
				'tax_query' => array(
					array(
						'taxonomy' => $term->taxonomy,
						'field'    => $term->slug,
						'terms'    => $term->term_id
					)
				)
			);
			$loop = new WP_Query( $args );

			if ( $loop->have_posts() ) :
				while ( $loop->have_posts() ) : $loop->the_post(); ?>
                    <h3><?php the_title(); ?> </h3>
                    <?php the_excerpt(); ?>
					<?php
				endwhile;
			endif; wp_reset_query();
			?>
		</div>
	<?php endforeach; ?>
</div> ```
