/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
struct Comp{
public:
    bool operator()(const Point& p1, const Point& p2 ) const
    {
        return p1.x < p2.x || (p1.x==p2.x&&p1.y<p2.y);
    }
};
class Solution {
public:
    int maxPoints(vector<Point>& points) {
        if ( points.size() < 3 ) return points.size();
        
        // store each unique points and its frequency
        map<Point,int, Comp> counts;
        for(auto &p : points) ++counts[p];
        
        
        int maxp = 0;
        for( map<Point,int,Comp>::const_iterator i = counts.begin(); i !=counts.end(); ++i)
        {
            const Point& p1 = i->first;
            int cur_p = i->second;
            
            // advance to next iterator 
            auto second = i;
            ++second;
            
            // p1 is the last unique point 
            if ( second == counts.end() )
            {
                maxp = max(maxp,cur_p);
                break;
            }
            
            const Point& p2 = second->first;
            cur_p += second->second; // current line contain freq(p1) + freq(p2) points
            
            
            bool is_vertical = (p1.x==p2.x);
            float slope = ( is_vertical ) ? 0 : float((p2.y-p1.y))/(p2.x-p1.x);
            
            ++second;
            // p2 is the last unique point
            if ( second == counts.end() )
            {
                maxp = max(maxp,cur_p);
                break;
            }
            
            for( auto j = second; j != counts.end(); ++j )
            {
                const Point& p = j->first;
                
                
                if ( is_vertical )
                {
                    // vertical line we check only x axis
                    if ( p.x == p1.x ) cur_p += j->second;
                }
                else
                {
                    if ( p.x != p1.x && fabs( float((p.y-p1.y))/(p.x-p1.x) - slope ) < 0.0000000001 )  // avoid float problem 
                    {
                        cur_p += j->second;
                    }
                }
            }
            
            maxp = max(cur_p, maxp);
            
        }
        return maxp;
    }
};
