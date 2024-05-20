# name: my-lottery-plugin
# about: A plugin to conduct lottery draws in Discourse
# version: 0.1
# authors: joo2017
# url: https://github.com/joo2017/my-lottery-plugin

enabled_site_setting :my_lottery_plugin_enabled

after_initialize do
  if defined?(DiscourseAutomation)
    DiscourseAutomation::Scriptable::LOTTERY_SCRIPT = "lottery_script"
    
    add_automation_scriptable(DiscourseAutomation::Scriptable::LOTTERY_SCRIPT) do
      field :reward, component: :text, required: true
      field :announcement_post, component: :post, required: true

      version 1
      triggerables [:recurring, :point_in_time]

      script do |context, fields, automation|
        post = Post.find(context['post_id'])
        commenters = post.comments.pluck(:user_id).uniq
        winner = User.find(commenters.sample)
        
        # 宣布中奖者
        PostCreator.create!(
          Discourse.system_user,
          topic_id: fields.dig("announcement_post", "value"),
          raw: "恭喜 @#{winner.username} 中了本次抽奖！奖品是: #{fields.dig("reward", "value")}"
        )
      end
    end
  end
end
