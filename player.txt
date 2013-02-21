class Player
  def play_turn(warrior)

    @health = warrior.health if !@health
    @cautionhealth = 10
    @lookfor = warrior.look
    @moveback = 2 if !@moveback


    if (@lookfor[0].to_s == "Captive" || @lookfor[1].to_s == "Captive") && !warrior.feel.captive? && warrior.feel.empty?
      puts "feels Captive!"
      warrior.walk!
    elsif warrior.feel.captive?
      puts "Rescue the Captive!"
      warrior.rescue!
    elsif warrior.feel(:backward).captive?
      "Feel a Captive backward, need to pivot to rescue"
      warrior.pivot!
    elsif ((@lookfor[1].to_s == "Wizard" || @lookfor[2].to_s == "Wizard") || (@lookfor[1].to_s == "Archer" || @lookfor[2].to_s == "Archer")) && @lookfor[0].empty?
      "Shooting Archer"
      warrior.shoot!
    elsif warrior.health < @cautionhealth && @moveback > 0 && !warrior.feel.wall?
      "Dying, need to move back"
      warrior.walk!(:backward)
      @moveback = @moveback - 1
    elsif warrior.feel.empty? && warrior.health == @health && warrior.health < 20
      "Resting"
      warrior.rest!
    elsif warrior.feel.empty?
      warrior.walk!
    elsif warrior.feel.wall?
      "There's a wall, need to pivot"
      warrior.pivot!
    elsif !warrior.feel.empty?
      "Attaking enemies!"
      warrior.attack!
    end
  
    @health = warrior.health
    @moveback = 2 if warrior.health == 20
  end

end
